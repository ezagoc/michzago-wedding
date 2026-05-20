# agent handoff — boda michelle & eduardo

## stack
- django 4.2, sqlite (dev) / postgresql (prod via dj-database-url)
- bootstrap 3.3.6, whitenoise for static files, gunicorn
- fonts: cormorant garamond (headers/display) + jost (body) via google fonts
- deployed on railway: https://web-production-1ae1da.up.railway.app

## repo
- github: https://github.com/ezagoc/michzago-wedding
- original fork: https://github.com/russeaap/django-wedding (django 1.x — heavily patched for python 3 / django 4.x compat)

## railway config
- start command: `python manage.py collectstatic --noinput && python manage.py migrate && gunicorn bigday.wsgi`
- env vars: `SECRET_KEY`, `DEBUG=False`, `ALLOWED_HOSTS=*`
- no postgresql added yet — running sqlite in production (fine for a wedding, but db resets on redeploy)

## key files
| file | what it does |
|---|---|
| `bigday/settings.py` | main config — reads SECRET_KEY, DEBUG, ALLOWED_HOSTS from env |
| `bigday/templates/home.html` | main page layout — sections: welcome, carousel, event, gifts, contact |
| `bigday/templates/partials/event.html` | ceremony details (date, venue, dress code, etc.) |
| `bigday/templates/partials/gifts.html` | bank account info for gifts |
| `bigday/static/bigday/css/creative.css` | all custom styles — black & white theme, fonts, header, carousel |
| `bigday/static/bigday/images/` | all images (inicio.jpg = header, progressive1-4.jpg = carousel) |
| `guests/models.py` | Party + Guest models for rsvp system |
| `guests/views.py` | dashboard, rsvp, guest list, export |

## rsvp system
- admin: `/admin` — create parties and guests here
- each party gets a unique invite link: `/invite/<uuid>/`
- share that link per whatsapp/email — guests confirm from there
- dashboard (login required): `/dashboard/`
- guest list export (login required): `/guests/export`
- to create superuser on railway: open railway shell → `python manage.py createsuperuser`

## content to still update
- `gifts.html` — replace "numero de cuentas" with real bank account numbers
- venue — update event.html when location is confirmed
- email settings in `settings.py` if sending invitations via email

## python 2 → 3 fixes applied
- `print` statements → `print()`
- `django.core.urlresolvers` → `django.urls`
- `django.conf.urls.url` → `django.urls.re_path`
- `StringIO` → `io.StringIO`
- `raw_input` → `input`
- `NullBooleanField` → `BooleanField(null=True)`
- `MIDDLEWARE_CLASSES` → `MIDDLEWARE`
- `{% load staticfiles %}` → `{% load static %}` (10 templates)
- `dict.keys().remove()` → `list(dict.keys()).remove()`
- `ForeignKey(Party)` → `ForeignKey(Party, on_delete=models.CASCADE)`
