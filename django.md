 Django

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

## Queryset

### Methods

#### `bulk_update`

Efficiently updates given fields on provided model instances generally with one SQL query. Returns the number of updates objects.

This is more efficient than iterating through a list of model instances and calling `save()` on them but there are couple things to remember:
- `save()` isn't called internally so `pre_save` and `post_save` signals aren't sent
- use `batch_size` to avoid generating too big of an SQL query
- when a batch contains duplicates, only first instance is updated

#### Syntax

```python
bulk_update(objs, fields, batch_size=None)
``` 

##### Example

```bash
>>> objs = [
...     Entry.objects.create(headline="Entry 1"),
...     Entry.objects.create(headline="Entry 2"),
... ]
>>> objs[0].headline = "This is entry 1"
>>> objs[1].headline = "This is entry 2"
>>> Entry.objects.bulk_update(objs, ["headline"])
2
```

## Logging

Instead of using `print()`, it is possible to debug what's happening in Django with loggers. Django uses Python's builtin `logging` module.

Logging in Python and Django involves four different parts:

1. Loggers
2. Handlers
3. Filters
4. Formatters

### Loggers

Lorem ipsum.

### Handlers

Lorem ipsum.

### Filters

Lorem ipsum.

### Formatters

Lorem ipsum.

### Propagation

Loggers are named hierarchically like modules are. So there can be both `django` and `django.server` loggers defined in a logging configuration. To send a log record, a logger instance needs to be ontained first by calling `logging.getLogger(name)`. When you provide a name for the logger, for example `logging.getLogger("django.server")`, log records are sent to the `django.server` logger in the current file. `django.server` will process the log record first but then it will get *propagated* to its parents in the logger hierarchy, `django` on this case, which will also handle the record with its handlers. 

Loggers can have a `propagate` setting set as `False` (defaults to `True`) which blocks the propagation.

### Logging configuration

When a Django application is spun up via WSGI or ASGI, `django.setup()` function is called. The function setups logging and installed apps configurations. For logging purposes, the interesting part is

```python
def setup(set_prefix=True):
    ...
    from django.conf import settings
    ...
    from django.utils.log import configure_logging
    
    configure_logging(settings.LOGGING_CONFIG, settings.LOGGING)
    ...
```

By default, `settings.LOGGING_CONFIG` and `settings.LOGGING` have the following values:

```shell
>>> from django.conf import settings
>>> settings.LOGGING_CONFIG
'logging.config.dictConfig'
>>> settings.LOGGING
{}
```

Django uses `dictConfig` format for configuring logging. `dictConfig` comes from Python's standard library's `logging` module as a function which handles the whole configuration process. In Django, logging configuration can come from other places and formats than `dictConfig` but that is the default one. The `configure_logging()` imports the logging configuration function that has been set for the Django project. Then, Django configures default logging configurations with `dictConfig` and `DEFAULT_LOGGING`, and then extends/overrides the configurations with custom settings, `logging_settings`.

```python
# Default logging for Django. This sends an email to the site admins on every
# HTTP 500 error. Depending on DEBUG, all other log records are either sent to
# the console (DEBUG=True) or discarded (DEBUG=False) by means of the
# require_debug_true filter. This configuration is quoted in
# docs/ref/logging.txt; please amend it there if edited here.
DEFAULT_LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "filters": {
        "require_debug_false": {
            "()": "django.utils.log.RequireDebugFalse",
        },
        "require_debug_true": {
            "()": "django.utils.log.RequireDebugTrue",
        },
    },
    "formatters": {
        "django.server": {
            "()": "django.utils.log.ServerFormatter",
            "format": "[{server_time}] {message}",
            "style": "{",
        }
    },
    "handlers": {
        "console": {
            "level": "INFO",
            "filters": ["require_debug_true"],
            "class": "logging.StreamHandler",
        },
        "django.server": {
            "level": "INFO",
            "class": "logging.StreamHandler",
            "formatter": "django.server",
        },
        "mail_admins": {
            "level": "ERROR",
            "filters": ["require_debug_false"],
            "class": "django.utils.log.AdminEmailHandler",
        },
    },
    "loggers": {
        "django": {
            "handlers": ["console", "mail_admins"],
            "level": "INFO",
        },
        "django.server": {
            "handlers": ["django.server"],
            "level": "INFO",
            "propagate": False,
        },
    },
}


def configure_logging(logging_config, logging_settings):
    if logging_config:
        # First find the logging configuration function ...
        logging_config_func = import_string(logging_config)

        logging.config.dictConfig(DEFAULT_LOGGING)

        # ... then invoke it with the logging settings
        if logging_settings:
            logging_config_func(logging_settings)
```

This setup gives the possibility to add own configuration functions and configurations on top of Django's defaults.

#### Defaults

The default logging configuration includes preset loggers, handlers, formatters and filters.



## Settings

Lorem ipsum.
