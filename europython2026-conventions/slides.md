---
theme: seriph
title: I Am a Sucker for Conventions
info: |
  EuroPython 2026 · Kraków · 15 July 2026 · Chamber Hall B (S3B) · 30 min.
  Why Django's defaults work, until they don't — told through Django's
  extension points, a 2010-era monkey patch, and the negotiations still
  happening in 2026.
colorSchema: light
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: view-transition
mdc: true
layout: intro
---

<p class="kicker"># europython-2026 · kraków · 2026-07-15 </p>

<h1 class="mt-10">i am a sucker<br>for conventions<span class="cursor"></span></h1>

<p class="mt-6 opacity-80">why django's defaults work, until they don't.</p>

<EpFooter />

<!--
Speaker notes — 0:30
- Walk on. Wait a beat.
- "I am a sucker for conventions." Let that line breathe.
- Don't apologise for it. It's the through-line of the next 30 minutes.
-->

---
layout: default
hideFooter: true
---

# <span style="color: var(--ep-green)">$</span> whoami

<div class="grid grid-cols-2 gap-10 mt-4">
<div class="kv">
<div class="label">[code]</div>
<div>python&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <strong>20+ years</strong></div>
<div>django&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <strong>since 0.96 (2007)</strong></div>
<div>day job &nbsp;&nbsp;&nbsp;= <strong>boostinc.com</strong></div>
<div>teaches&nbsp;&nbsp;&nbsp;&nbsp;= python · django @ university</div>
<div>co-founder = <strong>evolutio.pt</strong></div>
</div>
<div class="kv">
<div class="label">[community]</div>
<div>djangocon.eu = <strong>co-organizer, 2020–<span style="display: inline-grid"><span v-click.hide="1" style="grid-area: 1 / 1">2025</span><span v-click="1" style="grid-area: 1 / 1">2027</span></span></strong><br/>
<span class="text-sm opacity-70">2020 · 2021 · 2022 porto · 2024 vigo · <br/>2025 dublin</span></div>
<div>pycon.pt&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <strong>founder, 2022→</strong><br/>
<span class="text-sm opacity-70">2022 porto · 2023 coimbra · 2024 braga · <br/>2025 lisbon · <strong>2026 aveiro, sep 3–5</strong><br><a href="https://2026.pycon.pt">2026.pycon.pt</a></span></div>
</div>
</div>

<div v-click="1" class="announce mt-6">

<strong>+</strong> djangocon europe 2027 → <strong>innsbruck, austria</strong>

</div>

<EpFooter />

<!--
Speaker notes — 1:00
- Don't read the lists — one line each.
- Land the announcement: this is the FIRST time DjangoCon Europe 2027 is said
  out loud anywhere. Innsbruck, Austria. Enjoy the beat.
- Same click also flips the kv line 2020–2025 → 2020–2027. Don't spoil it —
  glance at the kv column as it changes.
- Quick PyCon Portugal plug: Aveiro, 3–5 September, CFP and tickets live.
-->

---
layout: section
---

<p class="kicker"># act 1 of 3 — the hook</p>

# A confession

<!--
Speaker notes — 0:20
- Pivot from cold open into the personal frame.
- This whole talk is the answer to: why do I love conventions, and what do I do
  when they stop earning their keep?
-->

---

# I am a sucker for conventions

<v-clicks>

- They simplify my day-to-day
- They reduce cognitive load
- They let new teammates know what's where
- They make code reviews shorter
- They free me up to think about the actual problem

</v-clicks>

<div v-click class="mt-10 opacity-80">
Conventions are <strong>cached decisions</strong> — someone smart already thought about it,
so I don't have to.
</div>

<!--
Speaker notes — 0:45
- Click through. Land "cached decisions" — that's the framing I'll come back to.
- Audience self-recognition moment: most senior Django devs feel this.
-->

---

# Django ships with great cached decisions

```bash
django-admin startproject mysite
python manage.py startapp polls
```

A new dev opens any Django project and **immediately knows**:

- where settings live
- where URLs live
- where models live
- how to run the dev server
- how migrations work
- how to add an admin page

<div class="mt-6 opacity-80">
That's a <em>massive</em> productivity gift. You'd notice if it were gone.
</div>

<!--
Speaker notes — 0:30
- This isn't a Django sales pitch — it's setup. We need to feel the weight of
  what conventions give us before we touch the second half of the talk.
-->

---
layout: section
---

<p class="kicker"># act 1 of 3 — the turn</p>

# But every convention is a decision

<!--
Speaker notes — 0:15
- Beat. Look at the audience. This is the pivot.
-->

---
class: diff
---

<p class="kicker"># every convention is a decision</p>

# Every decision has trade-offs

<v-clicks>

- **Who** decided this convention?
- **When** did they decide it?
- **What problem** were they solving?
- **Does that problem still apply to me?**
- **What new problem** does the convention create as my project grows?
- A convention you can't defend is just a habit. {.minus}

</v-clicks>

<!--
Speaker notes — 1:00
- Don't moralise. Frame it as professional curiosity.
- Land "a convention you can't defend is just a habit." Pause.
-->

---

# The thesis

<div class="mt-12 mx-auto text-center text-2xl font-bold opacity-90">

Django's conventions earn their keep<br/>
because they come with <em>escape hatches</em>.

</div>

<div class="mt-12 opacity-80 text-center">

The art of senior Django work is knowing those hatches —<br/>
and recognising when one doesn't exist yet.

</div>

<!--
Speaker notes — 1:00
- This is the new through-line. Everything that follows is either an escape
  hatch (a sanctioned deviation) or a hack (because no hatch existed yet).
- The framework is good *because* it lets you disagree productively.
-->

---
layout: section
---

<p class="kicker"># act 2 of 3 — the official hatches</p>

# Escape hatches by design

<!--
Speaker notes — 0:15
- Section divider. We're about to do a tour. ~10 minutes.
- Pace fast — the point is *the existence* of the hatch, not a tutorial on each.
-->

---
zoom: 0.95
---

# Models — `AbstractUser` & `AUTH_USER_MODEL`

The textbook escape hatch. **Don't extend `auth.User` — swap it.**

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class User(AbstractUser):
    company = models.ForeignKey("Company", null=True, on_delete=models.SET_NULL)
    email   = models.EmailField(unique=True)
```

```python
# settings.py
AUTH_USER_MODEL = "accounts.User"
```

<v-clicks>

- `AbstractUser` for "I want the defaults + a few fields"
- `AbstractBaseUser` for "I want full control of the auth fields too"
- `get_user_model()` everywhere — never `from django.contrib.auth.models import User`

</v-clicks>

<div v-click class="mt-4 text-sm opacity-70">
Shipped in Django 1.5 (2013). We'll come back to what we did before this existed.
</div>

<!--
Speaker notes — 1:30
- Foreshadow the monkey-patch story here — "we'll come back to what we did
  before AUTH_USER_MODEL existed".
- Get the audience to nod: "yes, this is the right way."
-->

---

# ORM — custom Managers & QuerySets

The ORM ships a hatch most teams never open. `QuerySet.as_manager()`:

```python
class QuestionQuerySet(models.QuerySet):
    def published(self):
        return self.filter(pub_date__lte=timezone.now())
    def with_choice_count(self):
        return self.annotate(num_choices=Count("choice"))

class Question(models.Model):
    text     = models.CharField(max_length=200)
    pub_date = models.DateTimeField()
    objects  = QuestionQuerySet.as_manager()

# Both work — chainable, composable, single source of truth for "published"
Question.objects.published().with_choice_count()
Question.objects.filter(pub_date__year=2026).published()
```

<div class="mt-4 opacity-80">
Encapsulates domain queries in the model. Replaces 90% of the helper functions
that drift across views and shell scripts.
</div>

<!--
Speaker notes — 1:15
- Optional aside: "if you see `Question.objects.filter(pub_date__lte=...)` in
  three views, that's a `.published()` waiting to happen."
- If asked: `models.Manager.from_queryset(QuestionQuerySet)` is the long form —
  same result, useful when mixing into an existing Manager subclass.
-->

---
zoom: 0.92
---

# Admin — `ModelAdmin` overrides

The admin's "register and forget" is the surface. Underneath, every part is a hook.

```python
@admin.register(Question)
class QuestionAdmin(admin.ModelAdmin):
    list_display       = ("text", "owner", "pub_date")
    list_select_related = ("owner",)   # fix N+1 — one line

    def get_queryset(self, request):
        qs = super().get_queryset(request)
        if request.user.is_superuser:
            return qs
        return qs.filter(owner=request.user)   # row-level filtering, blessed

    def save_model(self, request, obj, form, change):
        if not change:                         # creation only
            obj.owner = request.user           # inject request context into the save
        super().save_model(request, obj, form, change)
```

<div class="mt-2 text-sm opacity-70">

Also: custom actions (<code>@admin.action</code>), <code>get_form</code>,
<code>formfield_for_foreignkey</code>, <code>readonly_fields</code>,
<code>inlines</code>, custom <code>AdminSite</code>.

</div>

<!--
Speaker notes — 1:30
- The audience knows `list_display` and `inlines`. The point is to remind them
  that `get_queryset`, `save_model`, `get_form`, and custom actions are part of
  the same API surface and very cheap to override.
- `save_model` is the hook I reach for constantly — inject request context into
  every save. The same bend exists at the model layer via a `save()` override;
  if you do that, *always* call `super().save()`. Bugs from skipping it are
  still common.
-->

---
zoom: 0.95
---

# Views & Forms — lifecycle hooks

CBVs and Forms are an inheritance API. Every named method is a planned hook.

```python
class QuestionListView(ListView):
    def get_queryset(self):
        return super().get_queryset().published()   # the custom QuerySet, reused

class QuestionCreateView(CreateView):
    form_class = QuestionForm

    def form_valid(self, form):                     # FormView-family hook
        form.instance.owner = self.request.user
        return super().form_valid(form)
```

```python
class QuestionForm(forms.ModelForm):
    def __init__(self, *args, user=None, **kwargs):
        super().__init__(*args, **kwargs)
        if user and not user.is_staff:           # dynamic choices per user
            self.fields["category"].queryset = Category.objects.public()
```

<div class="mt-2 text-sm opacity-70">

Also: <code>get_context_data</code>, <code>get_form_kwargs</code>, <code>dispatch</code> — and every mixin hook (<code>test_func</code>, …).

</div>

<!--
Speaker notes — 1:15
- Senior audience: you've all done these. The point is that they are
  *the framework's API*, not workarounds.
- `form_valid` lives on the FormView family (`CreateView`/`UpdateView`) — hence
  the second class. It never fires on a plain `ListView`.
- The Form __init__ pattern in particular is officially supported and one of
  the cleanest places to inject runtime context.
-->

---
zoom: 0.88
---

# Auth — custom backends

Authentication backends are a clean extension contract: implement
`authenticate()` + `get_user()`, register in settings.

```python
# accounts/backends.py
from django.contrib.auth import get_user_model
from django.contrib.auth.backends import ModelBackend

class EmailBackend(ModelBackend):
    def authenticate(self, request, username=None, password=None, **kwargs):
        User = get_user_model()
        try:
            user = User.objects.get(email__iexact=username)
        except User.DoesNotExist:
            return None
        if user.check_password(password) and self.user_can_authenticate(user):
            return user
```

```python
# settings.py — stacked; Django tries them in order
AUTHENTICATION_BACKENDS = [
    "accounts.backends.EmailBackend",
    "django.contrib.auth.backends.ModelBackend",
]
```

<!--
Speaker notes — 1:00
- Senior audience: most of you have done this for SSO/LDAP/SAML. Worth saying
  out loud that the backend API is *the* officially sanctioned deviation.
-->

---

# There are dozens more

<div class="dozens text-sm mt-2">

- **Abstract base models** — share fields, no extra table
- **Proxy models** — same table, different Python
- **`Model.save()` overrides** — always call `super().save()`
- Custom **Field** types (`to_python`, `from_db_value`)
- Custom **Storage** backends (`STORAGES["default"]`)
- **Middleware** with deliberate ordering
- **Template tags & filters** (`{% load %}`)
- **Admin template overrides**
- **Context processors**
- Custom **URL converters** (`<slug:>`, your own)
- **`AppConfig.ready()`** for signal wiring
- **Signals** — `post_save`, `setting_changed`, your own
- **Migrations** — `RunSQL`, `RunPython`
- **Database routers** for multi-DB
- **`managed = False`** for legacy schemas
- Custom **management commands**
- Custom **test runner** (`DiscoverRunner`)

</div>

<div class="mt-6 opacity-80">

Each one is a place Django <em>expected</em> you might disagree —<br/>
and built a door for you to walk through.

</div>

<!--
Speaker notes — 0:30
- Don't read the list. Glance over it, say "the docs page on extension points
  is a half-day read and the best half-day a senior Django dev can spend."
-->

---
layout: section
---

<p class="kicker"># act 2 of 3 — the monkey-patch years</p>

# When there's no hatch

<!--
Speaker notes — 0:15
- Pivot to the story. ~7 minutes here.
- This is the personal section.
-->

---
zoom: 0.9
---

<p class="kicker"># when there's no hatch</p>

# Django, 2010

`auth.User` was **not swappable**. It had:

- `username` — required, max length 30
- `first_name`, `last_name` — anglocentric, blank-allowed
- `email` — not unique
- `is_staff`, `is_superuser`, `is_active`
- `password`, `last_login`, `date_joined`

That's it. No extension point, no `AUTH_USER_MODEL` setting.

<div class="mt-6 opacity-80">

If you wanted login-by-email, or any extra field on the user, you had two
choices:

- a separate `Profile` model with a `OneToOneField` to `User` (ugly joins everywhere)
- **monkey-patch `User` directly**

</div>

<!--
Speaker notes — 0:45
- Set the scene. The audience needs to feel the constraint before the hack
  becomes interesting.
-->

---
zoom: 0.88
---

<p class="kicker"># when there's no hatch, you make one</p>

# The `ravelsoft patchmodel` pattern (2010)

```python
# Adapted from ravelsoft.com/blog/2010/patchmodel-hacky-solution-extending-authuser-class
from django.contrib.auth.models import User

class UserOverride:                       # old-style class — intentional
    email   = models.EmailField(_("e-mail"), unique=True)
    company = models.ForeignKey("Company", null=True)   # no on_delete — 2010!

    def save(self, *args, **kwargs):
        # custom pre-save logic
        self.save__overridden(*args, **kwargs)   # original save(), renamed

patch_model(User, UserOverride)
```

<v-clicks>

- Reaches into `User._meta.local_fields`, pops the original `email` field
- Re-adds the new one with the original's `creation_counter` (column order preserved)
- Replaces methods, keeps the original under `<name>__overridden`

</v-clicks>

<div v-click class="mt-4 text-sm opacity-70">

It was, in the author's own words, a <strong>hacky solution</strong>.
But it worked. And in 2010 it was what we had.

</div>

<!--
Speaker notes — 1:30
- This is where it gets real for the senior part of the audience. Anyone who
  was writing Django before 2013 used some variant of this.
- "Old-style class" is a deliberate detail — Python 2 old-style class
  semantics; the patcher walks `__dict__` directly. Tell the audience why.
- Also period-accurate: no `on_delete`. It only arrived in Django 1.3 (2011)
  and only became required in 2.0. Mention it if anyone squints.
-->

---
zoom: 0.88
---

# I used it. For everything.

<v-clicks>

- The obvious fields — a unique `email`, `company`, `locale`, `avatar`
- The convenience methods — `full_name()`, `can_see(obj)`, `colleagues_of()`
- And the big one: **row-level permissions on `User` and `Group`**

</v-clicks>

<div v-click class="mt-4">

```python
class UserPermsOverride:
    def has_row_perm(self, perm, obj):
        if self.is_superuser:
            return True
        ct = ContentType.objects.get_for_model(obj)
        return RowPermission.objects.filter(
            content_type=ct, object_id=obj.pk,
            permission__codename=perm,
        ).filter(Q(user=self) | Q(group__in=self.groups.all())).exists()

patch_model(User,  UserPermsOverride)
patch_model(Group, GroupPermsOverride)   # same idea, group-side
```

</div>

<div v-click class="mt-4 opacity-80">
The Django convention was "permissions are per-model, not per-row."<br/>
My problem demanded per-row. The framework had no hatch. I made one.
</div>

<!--
Speaker notes — 1:30
- Personal. This is where the audience trusts you because you're not selling
  the hack — you're telling them about the day the framework didn't fit.
- The shape of RowPermission (ContentType + object_id + permission, user OR
  group) is exactly what django-guardian later shipped. Good aside:
  "in hindsight, I had written a worse django-guardian."
-->

---

# What happened next — two different fates

<div class="grid grid-cols-2 gap-4 mt-4">
<div>

#### The User patch

**Django 1.5, February 2013** —<br/>
`AUTH_USER_MODEL` arrives.

The hack becomes obsolete. The official escape hatch is in.

I migrated. The patch went away.

</div>
<div>

#### The row-level perms patch

**Django still has no first-class row-level permissions** — in 2026.

The community filled it instead:

- `django-guardian` (per-object perms, DB-backed)
- `django-rules` (predicate-based, no DB)
- DRF object-level permission classes

</div>
</div>

<div class="mt-6 opacity-80">

One hack got absorbed into core. The other got absorbed by the community.<br/>
Both stories <em>end well</em>. Neither ended with my code still in production.

</div>

<!--
Speaker notes — 1:30
- This is the punchline of the section. Both deviations were warranted, both
  eventually had a home, but you had to *track the framework* to know when.
- Tee up the flip: "and it's not just my story — once you see the shape,
  you see it everywhere. Watch." Then hit the next four slides fast.
-->

---
layout: section
---

<p class="kicker"># act 3 of 3 — the negotiation</p>

# Conventions are negotiated, not fixed

<!--
Speaker notes — 0:15
- Pivot to the meta-frame. ~3 minutes.
-->

---

# It happened with migrations

<div class="mt-8 space-y-4 text-lg">

<div><strong>2009</strong> — schema changes are raw SQL and folklore. <strong>South</strong>, a third-party app, becomes the de facto convention.</div>

<div><strong>2013</strong> — the community <em>funds a Kickstarter</em> for South's author to rewrite it inside Django.</div>

<div><strong>2014</strong> — Django 1.7 ships migrations. South is retired — by its own author.</div>

</div>

<div class="mt-8 opacity-80">

Community hack → funded negotiation → absorbed into core.

</div>

<!--
Speaker notes — 0:10
- FLIP MODE for this and the next three: ~10 s each, no clicks on purpose.
  Headline + arc, keep walking. The repetition IS the point.
- Spoken line over the flips: "and it keeps happening…"
- If riffing: Django's deprecation policy is what makes this graceful — slow,
  deliberate, signposted.
-->

---

# It happened with forms

<div class="mt-8 space-y-4 text-lg">

<div><strong>2009</strong> — nobody wants to hand-write form HTML. <code>django-uni-form</code>, reborn as <strong>django-crispy-forms</strong>, becomes the answer.</div>

<div><strong>2017</strong> — Django 1.11 makes widget rendering template-based, adds <code>FORM_RENDERER</code>.</div>

<div><strong>2022–23</strong> — 4.1 ships div rendering; 5.0 ships field groups. Absorbed in slices — crispy still rides on top.</div>

</div>

<div class="mt-8 opacity-80">

Community hack → official hatch → still being absorbed.

</div>

<!--
Speaker notes — 0:10
- Flip. Don't explain — the years do the work.
-->

---

# It happened with `JSONField`

<div class="mt-8 space-y-4 text-lg">

<div><strong>2012</strong> — JSON lives in <code>TextField</code>s, with <code>json.loads()</code> at every boundary; <code>django-jsonfield</code> papers over it.</div>

<div><strong>2015</strong> — Django 1.9 adds <code>contrib.postgres.JSONField</code> — a hatch, if you run Postgres.</div>

<div><strong>2020</strong> — Django 3.1 ships cross-database <code>JSONField</code> in core.</div>

</div>

<div class="mt-8 opacity-80">

Hack → contrib hatch → first-class field.

</div>

<!--
Speaker notes — 0:10
- Flip.
-->

---

# It's still happening — composite keys

<div class="mt-8 space-y-4 text-lg">

<div><strong>2005</strong> — Trac ticket <strong>#373</strong> opens: <em>"add support for multiple-column primary keys."</em></div>

<div><strong>2005–2024</strong> — twenty years of workarounds: surrogate keys, <code>unique_together</code> contortions, third-party attempts.</div>

<div><strong>2025</strong> — Django 5.2 ships <code>CompositePrimaryKey</code>. The ticket closes.</div>

</div>

<div class="mt-8 opacity-80">

A twenty-year negotiation, closed last year. Every release is a negotiation made visible.

</div>

<!--
Speaker notes — 0:10
- Last flip — note the title's tense shift: *still* happening. Sets up Carlton.
- Beat before the next slide: "and sometimes the negotiation reopens…"
-->

---
class: quote-slide
---

<p class="kicker"># even the official path gets questioned</p>

> custom user models – whilst monstrously cool in their implementation via swappable models – are a failed experiment…

<p class="attribution">— carlton gibson · django fellow 2018–2023 · <em>"evolving django's auth.user"</em> · aug 2024</p>

<v-clicks>

- "…a complexity tax we all pay, a performance tax that we're invited to fall into…"
- The "highly recommended custom user model" might come out of the docs
- A future Django could ship a bolt-on `unique=True` on `auth.User.email`

</v-clicks>

<div v-click class="mt-6 opacity-80">

The escape hatch I migrated to in 2013 may itself become the new monkey patch.<br/>
That's not a failure of the framework. That's the negotiation working.

</div>

<!--
Speaker notes — 1:15
- This is the slide where the audience leans forward. Even the canonical
  answer is up for re-examination — and that's a feature, not a bug.
- Both quoted fragments are verbatim from the essay (31 Aug 2024) — safe to
  attribute even if Carlton is in the room. The full sentence, for the voice:
  "…they add a complexity tax we all pay, a performance tax that we're
  invited to fall into, and fail to address in practice the problems they
  were introduced to solve."
-->

---

# You are part of the negotiation

<v-clicks>

- Trac tickets and pull requests are open
- The **Django Forum** (Internals) is open — it replaced the `django-developers` list in 2025
- **DEPs** (Django Enhancement Proposals) are open
- The yearly release cycle is open
- **DjangoCon hallway conversations** are open

</v-clicks>

<div v-click class="mt-8 opacity-80">

The reason Django's conventions are <em>so good</em> is precisely because
they are <em>not handed down</em>. They're argued for, by people who use them,
in public.

</div>

<div v-click class="mt-4 opacity-80">

If you've been quietly bending a convention for two years and it works —<br/>
write the post. File the ticket. Give the talk.

</div>

<!--
Speaker notes — 1:00
- Personal tone. Hand a small action item to the audience.
- Aside worth landing: even the negotiation venue got renegotiated — the
  django-developers mailing list retired into the Forum in 2025. The thesis,
  proving itself.
-->

---
layout: section
---

<p class="kicker"># act 3 of 3 — the playbook</p>

# Bend, don't break

<!--
Speaker notes — 0:15
- Last framework slide before the close.
-->

---

# Three questions before you deviate

<v-clicks>

1. **What problem was the convention solving?**<br/>
   <span class="opacity-70">If you can't answer this, you don't get to deviate yet.</span>

2. **Is there an official escape hatch I haven't read yet?**<br/>
   <span class="opacity-70">Half of "I have to monkey-patch" turns out to be "I haven't found the right manager / backend / signal."</span>

3. **If there really is no hatch — what new problem am I creating?**<br/>
   <span class="opacity-70">And how will the next dev (or future me) find out it's there?</span>

</v-clicks>

<div v-click class="mt-8 opacity-80">

If you can't answer all three, you're not deviating — you're improvising.

</div>

<!--
Speaker notes — 1:30
- Take this slow. The three questions are the closest thing to a checklist
  takeaway in the whole talk.
-->

---

# When you do hack — leave a trail, set a watch

A hack in private is a trap for the next dev; a hack with no follow-up is a
debt that compounds silently.

<v-clicks>

- Document the **why**, in one obvious place — `accounts/backends.py`, not `utils.py`
- Surface it — README, onboarding doc, a lint rule if you can
- A `# TODO: drop this when Django ships X` comment is a contract with yourself
- Track the relevant Trac ticket, DEP, or third-party package — and the release notes
- Re-evaluate every major release — does the hatch finally exist?

</v-clicks>

<div v-click class="mt-5 opacity-80">

Rule of thumb: <strong>every deviation deserves a paper trail bigger than the change itself.</strong>

</div>

<div v-click class="mt-3 opacity-80">

In 2013 I had a one-line TODO on the `patch_model(User, ...)` line.<br/>
In 2014 I deleted that line. It paid for itself.

</div>

<!--
Speaker notes — 1:15
- Practical. This is the slide where senior devs nod.
- "A TODO with a trigger condition is the cheapest form of project memory."
- The paper-trail line is deliberately a bit excessive. It should be.
-->

---
class: diff
zoom: 0.9
---

<p class="kicker"># bend, don't break</p>

# Costs you should expect

<v-clicks>

- **New hires will ask about your deviation** — good. Write it down once, link forever. {.minus}
- **Stack Overflow won't help** the way it does for vanilla Django {.minus}
- **LLMs will confidently suggest the convention you deliberately abandoned** — every single time {.minus}
- **Tooling upgrades sometimes fight you** — a plugin assumes the default; now you maintain the bridge {.minus}
- **You become responsible for the convention** — Django isn't carrying you here {.minus}

</v-clicks>

<div v-click class="mt-6 opacity-80">

You're trading <strong>familiar friction</strong> for <strong>local friction</strong>.<br/>
That trade is sometimes worth it. Sometimes it isn't. Be honest with yourself.

</div>

<!--
Speaker notes — 1:00
- Honesty slide. Don't sell deviation as free.
- The LLM point lands hard in 2026 — every team has felt this. Pause on it.
- This is also a soft warning to junior devs in the room — "don't take this talk
  as a license to invent."
-->

---
layout: section
---

<p class="kicker"># 30 seconds of day job</p>

# Aside — real-world context

<!--
Speaker notes — 0:10
- One small slide. Acknowledge the day job. Don't dwell.
-->

---

# Where I learned all this — slowly, painfully

<v-clicks>

- 2007–2013 — early Django, from **0.96** on: the `patchmodel` years
- 2013–onwards — the `AUTH_USER_MODEL` migration on a few real projects
- 2018–onwards — long-lived Django apps; the migration count climbs
- **2026** — 4.2 → 5.2 at **Boost Inc**: 10-year-old codebase, 4 PRs, **864 files**

</v-clicks>

<div v-click class="mt-6 opacity-80">

The Django framework's part of that 5.2 upgrade?<br/>
<strong>The smoothest part.</strong> The conventions still held — even the ones being questioned.

</div>

<div v-click class="mt-4 text-sm opacity-70">

Backward compatibility is also a convention. Django keeps that one religiously.

</div>

<div v-click class="mt-16 flex items-end gap-9">
  <img src="./images/boost-logo-green.png" class="h-8" alt="Boost Inc" />
  <span class="text-sm opacity-70">Thank you, Boost Inc, for backing my time here at EuroPython.</span>
</div>

<!--
Speaker notes — 1:00
- Small Boost Inc nod. Mention the upgrade briefly. Don't turn it into a
  retrospective talk — this is a side note.
- Story beats to riff on (pick ONE): of the 864 files, almost all were
  mechanical — the parts that fought back were pinned dependencies and
  tooling, not Django; the deprecation warnings had been announcing every
  break for two releases; the User model swap from 2013 sailed through
  untouched — three majors later.
- The "Django keeps backward compat religiously" line is a great way to close
  the section while still complimenting the framework.
- Land the thank-you: Boost paid for the time to be here. Short and warm.
-->

---
layout: section
---

<p class="kicker"># the landing</p>

# Takeaways

---

# Five things to take home

<v-clicks>

1. **Conventions are cached decisions.** They free your brain for the actual problem.

2. **Every Django default ships with an escape hatch.** Learn them; they're the senior-Django superpower.

3. **When the hatch doesn't exist, hacking is fine — with a paper trail and a watch.**

4. **Track the framework.** What was a hack in 2010 became a hatch in 2013 became a debate in 2024.

5. **Stay in the negotiation.** Django's conventions are good because people keep arguing about them — including you.

</v-clicks>

<!--
Speaker notes — 1:15
- Read them slowly. They're the closest thing to a recap; everything else has
  been narrative.
-->

---

# I still love Django

<div class="mt-8 text-xl opacity-90">

After nearly two decades of using it — since 0.96 — patching it where it didn't fit,
swapping its user model when it finally let me, and watching the community
question even the path I migrated to —

I love this framework <em>more</em>, not less.

</div>

<div class="mt-8 opacity-80">

Good conventions are the ones you can <strong>respectfully disagree with</strong>
and still want to keep using.

</div>

<div class="mt-8 opacity-80">

Django passes that test.

</div>

<!--
Speaker notes — 0:45
- Emotional close. Don't rush.
-->

---
layout: section
---

# Thank you

<div class="mt-8 opacity-80">

Questions, gripes, "but actually..." — all welcome.

</div>

<div class="mt-8 opacity-70 text-sm">

David Vaz · <a href="https://github.com/davidmgvaz">github.com/davidmgvaz</a> · @davidmgvaz<br/>
Slides: <a href="https://davidmgvaz.github.io/slides/europython2026-conventions/">davidmgvaz.github.io/slides/europython2026-conventions</a>

<br/>

References:

- The `patchmodel` hack — <a href="https://gist.github.com/davidmarble/1402045">gist.github.com/davidmarble/1402045</a> (adapted from ravelsoft.com, 2010)
- Carlton Gibson — <em>Evolving Django's auth.User</em> (Aug 2024)
- Row-level permissions — `django-guardian`, `django-rules`
- Django Enhancement Proposals — <a href="https://github.com/django/deps">github.com/django/deps</a>

</div>

<!--
Speaker notes — 0:30
- Stand still. Smile. Wait for the first hand.
- If no hands: "what's the convention *you* love to hate?" is a reliable opener.
-->
