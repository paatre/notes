# Django

## Templates

Templates are text files which are used to generate text-based format files. On Django, templates are primarily used to generate HTML files. Templates provide some programming logic such as variables, tags and filters which gives more power to otherwise similar HTML document constructing.

### Syntax

#### Variables

A template is rendered with a context that is a dictionary of keys and values. When a template is being rendered, variables are replaced with the value of the corresponding variable key in the context. Variables are defined in the template by surrounding the used variable with `{{` and `}}`.

#### Tags

Tags provide logic to the template rendering process. For example, you can iterate over a list that is passed to the template as a context variable with `{% for ... in ... %}` syntax. You could iterate over the list and create a list element of all the items in the list. There are many built-in tags in Django which can be referenced from [Django documentation](https://docs.djangoproject.com/en/4.2/ref/templates/builtins/#built-in-tag-reference). On top of that, custom tags can also be made.

#### Filters

Filters transform variable values and tag aruments. As with tags, there are many [built-in filters](https://docs.djangoproject.com/en/4.2/ref/templates/builtins/#built-in-filter-reference) in Django but custom filters can also be made. A good example is `default` filter which uses the given default value if the value used with the filter is falsy: `{{ value|default:"nothing" }}`.

### Engine

Django uses a template engine to load, render and handle templates. Django uses `django.template.backends.django.DjangoTemplates` backend which includes the template engine. This engine can be configured by the `TEMPLATES` setting.

#### Loaders

Template loaders are used for locating and loading templates. By default, when a Django project is started, it's shipped with two settings in the `TEMPLATES` setting: a list setting `DIRS` (`[]` by default) and a boolean setting `APP_DIRS` (`True` by default). When these variables are defined, two loaders will be automatically activated:
- `django.template.loaders.filesystem.Loader`
- `django.template.loaders.app_directories.Loader`

`filesystem.Loader` is a normal template loader which loads templates from directories specified by `DIRS` setting in `TEMPLATES`. `app_directories.Loader` is almost the same loader than the `filesystem.Loader`, but the only difference is that it will automatically go through app directories and tries to find a directory called `templates` in them. Because the `filesystem.Loader` comes first in the list of loaders, if it finds a matching template, it will be returned without going through the app directories. Which is why it should be considered where to put templates, especially overriding templates.

A quick note: if you add `loaders` in the `OPTIONS` setting, you can't also have `APP_DIRS`, it will [raise an ImproperlyConfigured exception](https://github.com/django/django/blob/main/django/template/engine.py#L43-L45).

#### Context processors

Django documentation says it best:

> Their main use is to add common data shared by all templates to the context without repeating code in every view.

A new Django project ships with four context processors by default on `OPTIONS`:
- `django.template.context_processors.debug`
- `django.template.context_processors.request`
- `django.contrib.auth.context_processors.auth`
- `django.contrib.messages.context_processors.messages`

The full information what variables these processors add to the context can be check from [the documentation](https://docs.djangoproject.com/en/4.2/ref/templates/api/#context-processors) but the most useful ones (at least for me at this point) seems to be `user`, `perms`, `sql_queries`, `request` and `messages`.

#### Built-ins

When using a tag or a filter which is not built-in in Django, it needs to be loaded first in the template with `{% load ... %}` tag. If the tag or a filter is included in the `builtins` list on `OPTIONS`, the tag or the filter can be used without loading it first. If the builtins is used, it does not override default built-ins by Django, it just adds the specied tags and filters to the default ones.
