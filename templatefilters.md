# Template Filters

We can extend the template engine by defining custom tags and filters using Python, and then make them available to your templates using the {% load %} tag.

> case : if you want to check conditions in template

### Code Layout

The app should contain a templatetags directory, at the same level as models.py, views.py, etc. If this doesn’t already exist, create it - don’t forget the __init__.py file to ensure the directory is treated as a Python package.

> Development server won’t automatically restart
> After adding the templatetags module, you will need to restart your server before you can use the > > tags or filters in templates.

> directory structure

```
polls/
    __init__.py
    models.py
    templatetags/
        __init__.py
        poll_extras.py
    views.py
```

And in your template you would use the following:

```python

{% load poll_extras %}

```

> templatetags/poll_extras.py

```python
# Sample template filter for check if a field is a RadioSelect

from django import template
from django.forms import RadioSelect

register = template.Library()

@register.filter
def is_radio(field):
    return field.field.widget.__class__.__name__ == RadioSelect().__class__.__name__
```

And here’s an example of how that filter would be used:

```html

{% load poll_extras %}
<!-- input\methodname -->
{% if form.gender|is_radio %}
    <p>Success</p>
{% endif %}
```
