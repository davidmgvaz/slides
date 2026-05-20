---
theme: seriph
title: Vendlive — Dev Workflow Improvements
info: |
  Pitch for the developer-workflow improvements that landed alongside the
  Django 4.2 → 5.2 upgrade on the INTERNAL-4719.
  Source: https://aeguana.atlassian.net/wiki/spaces/SD/pages/2540437516
class: boost-title
layout: default
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
fonts:
  sans: Inter
  serif: Raleway
  mono: JetBrains Mono
  weights: '400,500,600,700,800,900'
---

<div class="bt-row">
  <div class="bt-panel bt-left">
    <br/><br/>
    <img src="/boost-logo-green.png" class="bt-logo" alt="Boost inc" style="width:120px"/>
<br/><br/>
    <h1 class="bt-title">Vendlive<br/>Dev<br/>Workflow Improvements</h1>
    <p class="bt-meta bt-author"><span class="bt-name">David Vaz</span> · May, 2026</p>
  </div>
  <div class="bt-panel bt-right">
<br/><br/><br/>
    <p class="bt-sub">Developer-workflow changes that rode in with the Django 5.2 upgrade<br/><strong>not</strong> the upgrade itself.<br/><span class="bt-meta">All of it now on master.</span></p>
<p class="bt-meta"></p>
    <p class="bt-meta">INTERNAL-4719-django-52 4 PRs merged · 864 files changed</p>
  </div>
</div>

---
layout: default
class: boost-green-slide
---

# Why this deck

The Django 5.2 upgrade was the trigger, not the story.

While I was in there, a lot of small papercuts became obvious:

- no top-level command runner — rely on some bash scripts and personal aliases
- `README.md` was 364 lines, half of it stale
- onboarding was tribal knowledge
- pre-commit on staging hadn't been touched in a year and didn't match CI
- `pip-compile` is slow, and lots of dependencies were outdated
- Postgres 12 in dev, 12.18 in CI, both EOL, even if prod already runs 14
- branch DBs took minutes of migrations to spin up

This deck pitches **11 changes** that, together, should make day-to-day work noticeably less painful — all now on master via the four `django-52` PRs. I want feedback on what to keep, build on, or roll back.

---
layout: default
class: boost-dark-slide agenda-slide
---

# 11 pitches

1. One command, one source of truth
2. `just up` actually does what you'd expect
3. Local overrides without PR drama
4. Branch DBs in seconds, not minutes
5. Parallel worktrees that don't fight
6. Pre-commit = CI
7. Faster deps with `uv`
8. Workflow runbooks for risky ops
9. Team handbook in `.claude/` — skills + docs
10. A quiet quality net
11. Faster Sentry triage

<img src="/boost-machine.jpg" class="agenda-photo" alt="" />

---
layout: two-cols-header
---

# 1. One command, one source of truth

::left::

Every operation goes through **`just`**. Recipes are split by concern:

- `justfile` — backend-agnostic (`manage`, `shell`, `test`, `requirements`, ECS)
- `docker.justfile` — docker-only (`up`, `down`, `build`, `bash`, `db-*`)
- `local.justfile` — your personal recipes (gitignored)

Discover everything with `just --list`.

::right::

```bash
# Day to day
just up            # start everything
just down          # stop everything
just logs          # follow all services
just bash          # shell into Django container
just shell         # Django shell
just dbshell       # psql shell
just test          # full suite (parallel, --keepdb)
just test accounts.tests.test_login   # one module

# Dependencies
just requirements         # recompile, no upgrade
just requirements django  # bump just django
```

<div class="mt-4 text-xs opacity-70">
Rule in <code>CLAUDE.md</code>: never run <code>docker compose exec</code> or <code>python manage.py</code> directly. If a recipe is missing, add it.
</div>

`README.md` went **364 → 151 lines** — just badges, four steps to first run, and links to deeper docs. The new `CLAUDE.md` is the project map relying on `.claude/docs` for extra details.

---

# 2. `just up` actually does what you'd expect

Each container image gets tagged by a **hash of its build inputs**:

```just
image_hash := `cat docker/local/django/Dockerfile pyproject.toml uv.lock | shasum -a 256 | awk '{print substr($1, 1, 12)}'`
export VENDLIVE_IMAGE_TAG := "img-" + image_hash
```

```yaml
# docker-compose.yaml
x-common: &common
  image: vendlive-main:${VENDLIVE_IMAGE_TAG:-latest}
```

So:

- **Same deps as the last build?** `just up` reuses the cached image instantly.
- **Different `Dockerfile`, `pyproject.toml` or `uv.lock`?** `just up` rebuilds automatically.

No more "wait, did I forget to rebuild after pulling?".

<div class="mt-4 text-xs opacity-70">
Bonus: <code>just images-prune</code> drops every <code>vendlive-main</code> image except <code>latest</code> and the one matching current inputs.
</div>

---
layout: two-cols-header
---

# 3. Local overrides without PR drama

::left::

Every gitignored file has a tracked template in `examples/`:

```bash
cp examples/env                          .env
cp examples/env.local                    .env.local
cp examples/local.justfile               local.justfile
cp examples/docker-compose.override.yml  docker-compose.override.yml
cp examples/claude-settings.local.json   .claude/settings.local.json
cp examples/CLAUDE.local.md              CLAUDE.local.md
```

Explicit guidance in `examples/README.md`:

> **Commit it** if it would benefit a teammate.<br/>
> **Keep it local** if it's specific to you.

::right::

Common things you'll override per-machine:

```sh
# .env.local
DB_HOST=localhost                         # host postgres
VENDLIVE_PG_VERSION=12                    # if you need PG12
RABBIT_PORT=5673                          # parallel stack
ECS_STAGING_PROFILE=my-staging-alias      # custom AWS profile
```

```just
# local.justfile — your personal recipes
my-restore-prod-snapshot:
    aws s3 cp s3://… ./snap.dump
    just dbshell < ./snap.dump
```

When a local recipe proves useful for everyone, you graduate it into `justfile` (or `docker.justfile`).

---
layout: two-cols-header
---

# 4. Branch DBs in seconds, not minutes

::left::

The old way: `migrate` from empty on every fresh DB.<br/>
**~1500 migrations × ~4-5m** of waiting per branch.

The new way: a cached `pg_dump` template, restored on demand.

```bash
# Refresh the template — branch-agnostic, run from anywhere:
just db-template-refresh master

# Set up any branch's DB from the newest cached dump:
just db-fork master
```

`db-template-refresh` fetches the ref into a temp git worktree, migrates a throwaway DB, and dumps it to `~/.cache/vendlive/templates/`. `db-fork` is a `pg_restore` of that dump — seconds — then branch-specific migrations on top.

::right::

```text
| Situation                       | Recipe                 |
|---------------------------------|------------------------|
| New branch off master           | just db-fork master    |
| Verifying /promote staging      | just db-fork staging   |
| Master shipped new migrations   | just db-template-      |
|                                 |   refresh && db-fork   |
| InconsistentMigrationHistory    | just db-reset          |
| /merge-master added migrations  | just manage migrate    |
| Connection issue, schema OK     | just db-restart        |
| Cache dir growing               | just db-cache-prune    |
```

The dump cache is **shared across every worktree** on the machine — one refresh covers them all. `db-fork` is idempotent: a no-op when the dump hasn't changed.

<div class="mt-4 text-xs opacity-70">
Full rationale in <code>.claude/docs/db-workflow.md</code>.
</div>

---
layout: two-cols-header
---

# 5. Parallel worktrees that don't fight

::left::


```bash
wt 4720 start    # creates ../vendlive-4720, branch INTERNAL-4720
wt 4720 close    # cleans up when done
```

`wt` ships an `sh` bin and an `.envrc`, so direnv puts `wt` on your `PATH` automatically.

Three things make concurrent worktrees work:

- Per-worktree **container names** — not pinned `container_name:`/`image:`, so Compose generates them from the directory name
- Per-worktree **Postgres volume** — auto-namespaced from the directory basename
- Per-worktree **container ports** — override in `.env.local`


::right::

```diff
-postgres_data:   # one volume, every worktree fights for it
+postgres_data:
+  name: ${VENDLIVE_PG_VOLUME:-vendlive_postgres_data_pg16_vendlive-4720}
```

```diff
-  - "5672:5672"
-  - "15672:15672"
+  - ${RABBIT_PORT:-5672}:5672
+  - ${RABBIT_MGMT_PORT:-15672}:15672
+  - ${NODE_PORT:-8081}:8081
```

**Result:** `just up` in two worktrees side-by-side, both running tests, no clobbering.

<br/>

`wt` works off a **main checkout** (`vendlive/`); worktrees are sibling dirs `vendlive-<NNNN>`, each named for its Jira issue — `vendlive-4720` ↔ `INTERNAL-4720`.

::bottom::

<div class="mt-3 pt-3 text-sm opacity-80">

Built for Vendlive, but **not tied to it** — `wt` is repo-agnostic and works with any project. Try it on yours and tell me what breaks, help me improve it, show me alternatives I might have overlooked → [github.com/aeguana/worktree](https://github.com/aeguana/worktree)

</div>

---
layout: two-cols-header
---

# 6. Pre-commit = CI

::left::

`.pre-commit-config.yaml` reintroduced and **pinned to the same versions CI runs**:

- `ruff-check` + `ruff-format` (v0.15.0)
- `bandit` (1.8.3)
- `codespell` (2.4.1)
- `django-upgrade` targeting 5.2
- merge-conflict / trailing-whitespace / EOF / JSON / TOML / YAML / **detect-private-key**

Same lint locally → same lint in CI. No more "passes on my machine, red on Circle".

::right::

CI installs deps from the `uv.lock` with `uv sync`:

```yaml
- restore_cache:
    keys:
      - v1-python-deps-v3{{ checksum "uv.lock" }}
- run:
    name: Install python dependencies
    command: |
      UV_PROJECT_ENVIRONMENT=/usr/local \
        uv sync --frozen --inexact
```

`--frozen` means the lockfile is law — CI never silently re-resolves. Cache keys off `uv.lock`, so unchanged deps restore instantly.

<div class="mt-3 text-xs opacity-70">
CI &amp; local get the dev group too; staging &amp; production images add <code>--no-dev</code> to skip it (factory_boy, coverage, ruff, ipython…).
</div>

<div class="mt-3 text-xs opacity-70">
First-time setup is a one-liner: <code>pip install pre-commit &amp;&amp; pre-commit install</code>. With <code>wt</code> you only run it once, in the main checkout — worktrees share the same hooks dir.
</div>

---

# 7. Faster deps with `uv`

`uv` is the Astral-team replacement for `pip` and `pip-tools` — written in Rust, **10–100× faster** in practice.

Dependencies and the lockfile now live in `pyproject.toml` + `uv.lock`. The `requirements*.in` / `.txt` files are gone.

```diff
# docker/local/django/Dockerfile
-RUN pip install --no-cache-dir -r ./requirements_dev.txt
+RUN uv sync --frozen --inexact
```

The `just requirements` recipe wraps `uv lock`:

```bash
just requirements                     # re-resolve, keep current pins
just requirements --upgrade           # upgrade everything
just requirements twisted sentry-sdk  # upgrade specific packages
```

Plus the `/upgrade-requirements` slash command for safer guided bumps.

<div class="mt-6 px-4 py-3 rounded border-l-4 border-green-500 bg-green-50 text-sm text-black">
  <strong>One config file now:</strong> <code>pyproject.toml</code> absorbed
  <code>requirements.in</code>, <code>ruff.toml</code>, and the
  <code>bandit</code> / <code>codespell</code> / <code>coverage</code> settings —
  all <code>[tool.*]</code> sections in one place.
</div>

---
layout: two-cols-header
---

# 8. Workflow runbooks for risky ops

<div class="abs-br mb-4 mr-6 px-3 py-1 rounded-full text-xs font-mono bg-[#D97757] text-white shadow">.claude</div>

::left::

`.claude/commands/` codifies the "don't forget to..." steps for things that bite people:

| Command            | When                         |
|--------------------|------------------------------|
| `/prepare-commit`  | Lint + tests + draft message |
| `/merge-master`    | Pull master into your branch |
| `/promote staging` | Merge branch → staging       |
| `/ci`              | Debug a failed CircleCI run  |

<br/>

Each command is also a **readable runbook** — even without Claude, the markdown is the team's playbook.

::right::

**Highlight — cross-domain merge guard**

`/merge-master` and `/promote` read `DEVELOPER_ROLE` and flag files outside your domain:

```json
// .claude/settings.local.json
{ "env": { "DEVELOPER_ROLE": "backend" } }
```

| Role | Flagged for review |
|---|---|
| `backend` | `vue_frontend/**` |
| `frontend` | non-frontend code |
| `fullstack` | nothing — skipped |

<br/>

Claude flags cross-domain changes and asks before proceeding — you stay in control.

---
layout: two-cols-header
---

# 9. Team handbook in `.claude/`

<div class="abs-br mb-4 mr-6 px-3 py-1 rounded-full text-xs font-mono bg-[#D97757] text-white shadow">.claude</div>

::left::

Two folders that are useful **with or without an AI assistant** — they're team knowledge in version control, loaded on demand instead of bloating `CLAUDE.md`.

**`.claude/docs/`** — deep references

- `architecture.md` — apps, API namespaces, frontend
- `apis.md` — endpoint catalogue (29k of it)
- `db-workflow.md` — `db-fork` / template lifecycle
- `deployment.md` — CI/CD pipeline
- `service-versions.md` — pinned versions and where they live

::right::

**`.claude/skills/`** — opinionated how-tos

- `celery-patterns` — task design, scheduling, debugging
- `circleci-debugging` — CI red, local green
- `django-models` — ORM, migrations, N+1
- `django-testing-patterns` — TestCase, factory_boy, runner
- `drf-patterns` — base views, serializers, permissions
- `sentry-debugging` — triaging staging/prod errors

<div class="mt-4 px-3 py-2 rounded text-xs bg-gray-100 text-black">
  Skills double as concise pattern docs for humans. Read them as cheatsheets;
  Claude reads them as guidance. Either way, tribal knowledge stops being tribal.
</div>

---
layout: two-cols-header
---

# 10. A quiet quality net

<div class="abs-br mb-4 mr-6 px-3 py-1 rounded-full text-xs font-mono bg-[#D97757] text-white shadow">.claude</div>

::left::

A handful of low-noise hooks running in the background:

**Branch protection** — refuses Edit/Write on `main`. Forces a feature branch.

**Security guard** — blocks edits introducing footguns before they hit a diff:

```text
.raw(           .extra(         RawSQL
mark_safe(      @csrf_exempt    eval(
exec(           pickle.loads(   yaml.load(
os.system(      os.popen(       **request.POST.dict()
```

(Same list `CLAUDE.md` calls "ORM only, no raw, no extra" — now actually enforced.)

::right::

**Auto pre-commit** after every edit — runs the same hooks CI runs, fixes what it can in place.

**Auto-test on PR** — while running `/prepare-commit`, `/merge-master` or `/promote`, runs `just test` for files relevant to that commit/merge. No more hitting the wall on CI, no need to run full tests locally, only relevant ones.

**Secrets out of code:**

```diff
-GOOGLE_API_KEY = "AIzaSy..."
+GOOGLE_API_KEY = os.environ["GOOGLE_API_KEY"]
```

Same for `DJANGO_ENCRYPTED_FIELD_KEY`. Plus `pre-commit-hooks: detect-private-key` catches new ones.

---

# 11. Faster Sentry triage on staging

Every staging deploy now tags Sentry events with the **commit SHA that shipped them**:

```python
sentry_sdk.init(
    ...,
    release=os.environ.get("DEPLOY_COMMIT_SHA"),
)
```

So when a staging error fires, you can:

1. Click the release tag in Sentry
2. See exactly which commit introduced it
3. `git show <sha>` and you've got the diff

Replaces the "is this still happening, or was it before yesterday's deploy?" guesswork.

<div class="mt-6 px-4 py-3 rounded border-l-4 border-blue-400 bg-blue-50 text-sm text-black">
  <strong>Open question:</strong> a commit SHA pins the exact code, but it isn't a <em>version</em>. Should deploys carry a proper version number too? That premise is being explored on <code>INTERNAL-4802-version</code>.
</div>

<div class="mt-4 text-xs opacity-70">
Small change, but it's the one that pays back the most when staging/prod is acting up at 5pm on a Friday.
</div>

---
layout: default
class: boost-statement
---

<br/><br/>

# What I want from you

It's all on master — so this isn't "should we ship it", it's "is it pulling its weight":

- **What's earning its keep?** <br/>Tell me what you'd miss if it vanished — that's where we should invest.
- **What feels like over-engineering?** <br/>Honest signal welcome — better to roll something back than carry dead weight.
- **What's missing?** Papercuts I haven't hit yet.
- **The open question** — proper version numbers (slide 14). Worth doing? Impacts?

---
layout: default
class: boost-statement boost-thanks
---

<div class="thanks-text">
  <h1>Thanks</h1>
  <p>Questions, gripes, "why didn't you just…?" — all welcome.</p>
</div>

<img src="/vending.png" class="thanks-photo" alt="" />
