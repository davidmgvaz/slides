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
routerMode: hash
mdc: true
layout: intro
---

<p class="kicker"># europython-2026 · kraków · 2026-07-15 </p>

<h1 class="mt-10">i am a sucker<br>for conventions<span class="cursor"></span></h1>

<p class="mt-6 opacity-80">why django's defaults work, until they don't.</p>

<EpFooter />

<!--
clock 0:00 → 0:30 · 0:30

I am a sucker for conventions. 

That is not an apology; it is the whole talk.

For the next thirty minutes, I want to show you why Django's defaults have earned my loyalty for nearly two decades, and exactly what I do on the days they stop earning it.
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
<div>pycon.pt&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;= <strong>co-founder, 2022→</strong><br/>
<span class="text-sm opacity-70">2022 porto · 2023 coimbra · 2024 braga · <br/>2025 lisbon · <strong>2026 aveiro, sep 3–5</strong><br><a href="https://2026.pycon.pt">2026.pycon.pt</a></span></div>
</div>
</div>

<div v-click="1" class="announce mt-6">

<strong>+</strong> djangocon europe 2027 → <strong>innsbruck, austria</strong>

</div>

<EpFooter />

<!--
clock 0:30 → 2:30 · 2:00

Quickly, who am I?

Twenty-plus years of Python; Django since 0.96 back in 2007.

Before AUTH_USER_MODEL, before South, before class-based views, before LTS, almost from the beginning

These days, I am a senior Backend Developer at Boost Inc and teach Python and Django at university.

The community column matters a lot to me: 

I have co-organized DjangoCon Europe since 2020, the first virtual
edition, followed by Porto, Vigo, Dublin.

I co-founded PyCon Portugal. The fifth edition is this September in Aveiro; for the 3rd and 5th, you are more than welcome to join me. 

At PyCon Portugal, we pledge to host in a different city each year, which is a nice way to get to know our wonderful cities and to find local Python enthusiasts.

[click] And one more thing, which I can now announce: DjangoCon Europe 2027 is going to Innsbruck, Austria. With Jacob Rief as local organizer and Evolutio's team support, the same team that brought some of the previous editions. News will follow, hope to see you there.

A quick shout to all community members who want to host a Django Con Europe or PyCon Portugal; we need you. We can help in a number of ways. This year, at DjangoCon Europe in Athens, we simply share the knowledge with the local team to help them succeed. Next year, we will actually take the financial responsibility and be the core team together with Jacob. I am more than available to help you host a future DjangoCon Europe, wherever it is. Come and find me after if you are interested.
-->

---
layout: section
---

<p class="kicker"># act 1 of 3 — the hook</p>

# A confession

<!--
clock 2:30 → 2:50 · 0:20


So, the confession.

This whole talk is me answering one question: why do I love conventions, and what do I do when they stop earning their keep?
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
clock 2:50 → 3:25 · 0:35

I am a sucker for conventions because they carry my day.

[click] They simplify the boring decisions.

[click] They cut cognitive load.

[click] A new teammate opens the project and already knows where things live.

[click] Code reviews argue about the problem, not the layout.

[click] I can focus on the actual problem

[click] Here is the frame I will keep returning to: a convention is a cached decision.

Someone smart already thought about it, carefully, once, so I do not have to think about it every morning.
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
clock 3:25 → 3:50 · 0:25

As an over 20-year-old framework, 
Django is a museum of great cached decisions.

Two commands, and any Django developer on earth knows where settings live, where URLs live, how migrations work, how to get an admin.

That is a massive productivity gift, and like all infrastructure, invisible until it is gone.
-->

---
layout: section
---

<p class="kicker"># act 1 of 3 — the turn</p>

# But every convention is a decision

<!--
clock 3:50 → 4:05 · 0:15

But.

Every convention is a decision.
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
clock 4:05 → 4:50 · 0:45

And every decision has trade-offs

So let's interrogate them like the decisions they are.

[click] Who made this call? A core developer? A committee? Some blog post in 2009?

[click] And when? The web that decision was made for may not exist anymore.

[click] What hurt were they trying to stop, [click] and am I even feeling that hurt today?

[click] And the sneaky one: what does this convention start to cost once the project is ten years old?

STOP

These five questions are the difference between following a convention and merely obeying it.

Because, and this is the line to take home from this talk: 

[click] A convention you cannot defend is just a habit.
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
clock 4:50 → 5:35 · 0:45

Here is my actual thesis.

Django's conventions earn their keep because they come with escape hatches.

It is not that the defaults are perfect.

It is rather that the framework expects you to disagree with them, and builds doors for it. The framework is good because it lets you disagree productively.

The art of senior Django Developer work is knowing those hatches cold and hard, recognizing the day you are standing in front of a wall where a door should be.

Everything that follows is one of those two situations.
-->

---
layout: section
---

<p class="kicker"># act 2 of 3 — the official hatches</p>

# Escape hatches by design

<!--
clock 5:35 → 5:50 · 0:15

First: the doors that exist.

A fast tour.

The point is that these hatches exist, not a tutorial on each.
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
clock 5:50 → 7:00 · 1:10

The textbook example: the user model.

The convention is: do not touch auth.User, swap it.

[click] Subclass AbstractUser when you want the defaults plus your fields

[click] drop to AbstractBaseUser when you want to own authentication itself.

[click] Point AUTH_USER_MODEL at your class, and use get_user_model everywhere, never import User directly.

Notice the shape of this hatch, because Django repeats it everywhere: the escape hatch is a dotted path in settings.py.

[click] This one shipped in Django 1.5, in 2013.

Hold that date.

We will come back to what we did before it existed, and a story at the heart of this talk.
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
clock 7:00 → 7:55 · 0:55

The ORM's most underused hatch: custom QuerySets.

If the same filter call appears in three views, that is a published() method waiting to be born.

Subclass QuerySet, define your domain vocabulary, wire it with as_manager.

Chainable, composable, one source of truth for what published means.

This replaces most of the helper functions that drift around a codebase.

Simple, boring, official, and most teams never open the door.
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
clock 7:55 → 9:05 · 1:10

The admin looks like register and forget. One of the oldest features in Django, available to me since the beginning.

It is actually hooks all the way down. list_select_related kills an N plus one in one line. get_queryset scopes what each user sees: row-level filtering, fully blessed.

And save_model is the one I reach for constantly, injecting request context into every save: who created this, who touched it.

The same bend exists on the model's own save method; if you take it, call super, always.

People skip it, and the bugs are still boring and still common.

Plus actions, get_form, inlines, a whole AdminSite if you need one.

Every part is a hook.
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
clock 9:05 → 10:00 · 0:55

Class-based views sometimes feel like sushi; some love them, some hate them, 

At the beginning, they seem magical, but the named methods are a published API. get_queryset, reusing the QuerySet from two slides ago. form_valid on the create view, stamping ownership. `form_valid` lives in the FormView family (`CreateView`/`UpdateView`). It never fires on a plain `ListView`.

And forms: passing context into the constructor and narrowing choices per user is officially supported, and one of the cleanest injection points in the framework. The Form __init__ pattern in particular is officially supported and one of the cleanest places to inject runtime context.

These are not workarounds.

They are the framework's interface.

You've all done these. The point is that they are
*the framework's API*, not workarounds.
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
clock 10:00 → 10:45 · 0:45

Authentication backends: implement authenticate and get_user, register the dotted path, done.

Login by email in twenty lines.

They stack, and Django tries them in order.

Everyone who has wired SSO or LDAP has walked through this door. The backend API is *the* officially sanctioned deviation.

Again: the hatch is a string in settings.
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
clock 10:45 → 11:10 · 0:25

And there are dozens more.

Fields, storages, middleware, signals, routers, management commands.

I will not read the slide.

The docs page on extension points is a half day read, and it is the best half day a senior Django developer can spend.

The point: each of these is a place Django expected you to disagree, and built a door.
-->

---
layout: section
---

<p class="kicker"># act 2 of 3 — the monkey-patch years</p>

# When there's no hatch

<!--
clock 11:10 → 11:25 · 0:15

But sometimes, there is no door.
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
clock 11:25 → 12:00 · 0:35

Django, 2010. auth.User was not swappable.

Username: thirty characters.

First name, last name: anglocentric and half-useless.

Email: not unique.

If you wanted to log in by email, or any real field on the user, you had two options: 

A Profile model with a OneToOne and joins everywhere.

Or monkey-patch the User model.
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
clock 12:00 → 13:10 · 1:10

This is the ravelsoft patch_model pattern, 2010. I actually could not find the original post, nor am I sure this was the exact one I follow, but it was close enough for this talk anyway. 

And yes, that is an old-style class, on purpose (this still worked with Python 2): the patcher walks the class dict directly.

[click] It pops the original field out of User meta local_fields, 
[click] re-adds yours preserving the creation counter so column order survives, 
[click] and renames overridden methods so you can still call the original.

Notice also what is missing: no on_delete. It only arrived in Django 1.3 in 2011, and only became required in Django 2.0

It did not exist yet.

That is how old this code is.

[click] The author called it, his words, a hacky solution.

It was.

It also worked, and in 2010, it was what we had.
-->

---
zoom: 0.88
---

# I used it. For everything.

<v-clicks>

- The obvious fields — a unique `email`, `company`, `locales`, `avatar`
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
clock 13:10 → 14:20 · 1:10

And I used it.

For everything.

[click] The obvious fields: unique email, locales.

[click] Convenience methods.

[click] And the big one: row-level permissions, patched onto User and Group.

[click] Look at the shape of that query.

ContentType + object_id + permission: user or group.

If that looks familiar, it is django-guardian, five years early and considerably worse.

Again, there was a blog post explaining this idea, which I took inspiration from back then but can no longer find.

[click] Django's convention said permissions are per model.

My problem said per row.

There was no hatch.

So I hacked one.
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
clock 14:20 → 15:30 · 1:10

Here is what happened next, and it is the punchline of the section.

Two hacks, two different fates.

The user patch: Django 1.5 shipped AUTH_USER_MODEL in 2013.

The official door appears, I migrate, the patch dies.

The row-level patch: Django never shipped row-level permissions.

The community did.

Guardian, rules, DRF object permissions.

I migrated to those instead.

Both stories end well.

Neither ends with my code still in production, and that is the goal.

But you only get that ending if you are watching the framework.
-->

---
layout: section
---

<p class="kicker"># act 3 of 3 — the negotiation</p>

# Conventions are negotiated, not fixed

<!--
clock 15:30 → 15:45 · 0:15

Which brings us to the real frame: conventions are not handed down.

They are negotiated.

And once you see the shape, you see it everywhere. This was not my story after all...

Watch.
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
clock 15:45 → 15:55 · 0:10

Migrations.

Raw SQL folklore, then South becomes the convention, then the community literally crowdfunds its author to rewrite it into core.

Absorbed.
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
clock 15:55 → 16:05 · 0:10

Forms.

Nobody wants to write form HTML. crispy-forms carries a decade, and Django absorbs it in slices: template widgets, div rendering, field groups.
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
clock 16:05 → 16:15 · 0:10

JSONField.

TextFields full of json.loads, then a package, then a Postgres-only hatch, then core.

Hack to first class in eight years.
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
clock 16:15 → 16:25 · 0:10

And still happening: ticket 373, multiple column primary keys, opened in 2005, closed by CompositePrimaryKey in 5.2.

A twenty year negotiation.
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
clock 16:25 → 17:20 · 0:55

And even the official answers get re-opened.

Carlton Gibson, Django Fellow for five years, wrote in 2024 that custom user models are, his words, a failed experiment.

[click] A complexity tax we all pay, a performance tax we are invited to fall into.

Think about what that means.

[click]

[click]

[click] The hatch I migrated to in 2013, the fix for my monkey patch, is itself back on the table.

That is not the framework failing.

That is the negotiation working.

Nothing is sacred; everything is argued.
-->

---

# You are part of the negotiation

<v-clicks>

- Trac tickets and pull requests are open
- The **Django Forum** (Internals) is open — it replaced the `django-developers` list in 2025
- **DEPs** (Django Enhancement Proposals) are open
- The yearly release cycle is open
- **DjangoCon/EuroPython hallway conversations** are open

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
clock 17:20 → 18:05 · 0:45

And here is the part people forget: you are allowed in the argument.

[click] That ticket that annoys you? Anyone can open one, anyone can weigh in.

[click] The Forum is where the real debates happen now.

Fun fact: it replaced the django-developers mailing list last year, so even the negotiation venue got renegotiated.

[click] The big changes go through DEPs, written by people like us.

[click]

[click] And honestly, half of Django's direction gets argued in hallways like the one outside this room.

[click] That is exactly why the conventions are good: nobody handed them down.

They are argued about in public by the people who use them.

[click] So if you have been quietly bending a convention for two years and it works: write the post.

File the ticket.

Give the talk.
-->

---
layout: section
---

<p class="kicker"># act 3 of 3 — the playbook</p>

# Bend, don't break

<!--
clock 18:05 → 18:20 · 0:15

So how do you bend without breaking?

Three habits.
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
clock 18:20 → 19:30 · 1:10

First.

Three questions, before you deviate.

[click] One: do I understand what this convention was protecting me from?

If you cannot answer, you do not get to deviate yet.

[click] Two: did I actually go looking for the official door?

In my experience, half of I-need-to-monkey-patch dissolves into I-had-not-read-about managers, or backends, or signals.

[click] Three: if the hatch truly is not there, what does my workaround cost, and how will the next person even find out it exists?

[click] If you cannot answer all three, you are not deviating.

You are improvising.
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

In 2013 I had a one-line TODO on the `patch_model(User, ...)` line.<br/>
In 2014 I deleted that line. It paid for itself.

</div>

<div v-click class="mt-3 opacity-80">

Rule of thumb: <strong>every deviation deserves a paper trail bigger than the change itself.</strong>

</div>

<!--
clock 19:30 → 20:25 · 0:55

Second.

When you do hack, leave a trail and set a watch.

[click] Write down the why, and put it where the next person will trip over it — not in your head.

[click] Make it loud: a line in the README, a note in onboarding, a lint rule pointing at the docs.

[click] A TODO with a trigger condition, drop this when Django ships X, is a contract with yourself, and the cheapest project memory there is.

[click] Follow the ticket upstream, [click] and at every major release ask: does the hatch finally exist?

[click] My patch_model line had exactly one TODO on it.

In 2014 I deleted the line.

It paid for itself.

[click] Rule of thumb: every deviation deserves a paper trail bigger than the change itself.
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
clock 20:25 → 21:10 · 0:45

Third.

Price it honestly.

[click] New hires will ask.

[click] Official Docs, Stack Overflow, blog posts stop helping.

[click] LLMs, and this one lands in 2026, will confidently un-fix your deviation, every single time.

[click] Tooling assumes the default, so now you maintain the bridge.

[click] And you become responsible for the convention, because Django and Django Community are not carrying you anymore.

[click] You are trading familiar friction for local friction.

Sometimes worth it, sometimes not.

Be honest about which.
-->

---
layout: section
---

<p class="kicker"># 30 seconds of day job</p>

# Aside — real-world context

<!--
clock 21:10 → 21:20 · 0:10

Thirty seconds on my day job, because I should say where all this resonated with me.
-->

---

# How I learned all this — steadily

<v-clicks>

- 2007–2013 — early Django, from **0.96** on: fast moving, big updates each version, some monkey-patch
- 2013–2017 — slowly getting more stable, the `AUTH_USER_MODEL`, migrations
- 2018 onwards — The LTS Django era; the migration count climbs
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
clock 21:20 → 22:05 · 0:45

[click] Each version has new goodies, gotta get them all. The monkey-patch years when something was missing

[click] Getting better and better

[click] Then LTS, now I could breathe between LTS's

[click] and this year, at Boost, taking a ten-year-old codebase from 4.2 to 5.2.

Four PRs, 864 files. By the way, I also upgraded from Python 3.10 to 3.13 right after.

[click] And the framework's part of that upgrade?

The smoothest part. The one I knew better.

The conventions held, even the ones being questioned.

[click] Backward compatibility is a convention too, and Django keeps that one religiously.

[click] Which is why this thank-you matters: Boost is why I get to be here.

Thank you.
-->

---
layout: section
---

<p class="kicker"># the landing</p>

# Takeaways

<!--
clock 22:05 → 22:05 · passthrough

Let's land it.
-->

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
clock 22:05 → 23:00 · 0:55

Five things, and I will say them slowly.

[click] Conventions are cached decisions. Spend your thinking on your problem, not on plumbing.

[click] Every Django default ships with an escape hatch. Knowing them cold is what senior looks like in this framework.

[click] No hatch? Hack anyway — with a paper trail and a watch on the framework.

[click] And track it, because this story rhymes: the 2010 hack, the 2013 hatch, the 2024 debate.

[click] And stay in the negotiation.

The conventions are good because people keep arguing about them.

Be one of those people.
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
clock 23:00 → 23:35 · 0:35

After nearly two decades.

Patching it when it did not fit.

Swapping the user model when it finally let me.

Watching the community question the very path I migrated to.

I love this framework more, not less.

Good conventions are the ones you can respectfully disagree with and still want to keep using.

Django passes that test.
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
clock 23:35 → 24:05 · 0:30

That is it.

Questions, gripes, but-actually, all welcome, here or in the hallway.

Thank you.
-->
