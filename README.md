# copycat-ignite-django-project01

## Creating a Django project

### django-admin

To create a new project with Django, run the following commands:

```bash
python -m venv venv
source venv/bin/activate
python -m pip install Django
django-admin startproject project01 .
django-admin startapp myapp
```

> **Trick**: we pass "`.`" as an extra argument to allow picking the current directory as the project root.

Modify `settings.py` to include myapp in the list of installed apps, create `myapp/urls.py`, include it in `project01/urls.py`, create the home view in `myapp/views.py` and a `myapp/templates/index.html` template:

```python
# settings.py
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "myapp",
]


# myapp/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='home')
]


# project01/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path("", include("myapp.urls")),
    path("admin/", admin.site.urls),
]


# myapp/views.py
from django.http import HttpResponse
from django.template import loader

def index(request):
    template = loader.get_template('index.html')
    return HttpResponse(template.render())

```

```django
# myapp/templates/index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Django Project</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

### Installing the dependencies

If you just downloaded this project, run the following commands:

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

or alternatively, use the Makefile `install` script:

```bash
make install
```

### Running the project

Finally, execute the project with the command:

```
./manage.py runserver
```

or alternatively, use the Makefile `serve` script (after activating venv):

```bash
source venv/bin/activate
make serve
```

## Concepts

### "Components" and "Properties"

> Note: here's a reminder that I'm trying to reproduce (hence copycat) the concepts from [Rocketseat's React course](https://github.com/jobsonita/rocketseat-ignite-react-project01). This is not intended as an actual guide for web development in Django which has its own concepts and particularities.

A "component" is a template that acts like a complex html tag. It can receive "properties", and it returns a piece of html (with maybe some javascript?) code.

```django
# myapp/templates/post.html
<div>
    <strong>{{ author }}</strong>
    <p>{{ content }}</p>
</div>

# myapp/templates/index.html
...
<body>
  <div>
    {% include "post.html" with author="Me" content="Whatever I want to say" %}
    {% include "post.html" with author="You" content="Whatever you want to say" %}
  </div>
</body>
</html>
```

### Alternative to CSS Modules

Unfortunately, Django doesn't come with Scoped CSS or a CSS Modules-like solution like React does, so I'll be using [`django-sass`](https://pypi.org/project/django-sass/) to emulate that until the Ignite course eventually evolves into Tailwind (and then I'll try integrating Tailwind to this project).

We must run the following command to compile our `.scss` files:

```bash
./manage.py sass myapp/static/myapp/scss/ myapp/static/myapp/css/
```

In our `.html` files, we load the stylesheets:

```html
{% load static %}
<link href="{% static 'myapp/css/component.css' %}" rel="stylesheet">
```

While running the project on a terminal, we can watch the files for changes on a second terminal:

```bash
./manage.py sass myapp/static/myapp/scss/ myapp/static/myapp/css/ --watch
```

### Alternative to phosphor-react

Since I can't use that library on Django, I'll be using FontAwesome instead: https://fontawesome.com/docs/web/use-with/python-django
