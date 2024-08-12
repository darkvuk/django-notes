# Markdown Template Filter

```
pip install markdown
```

Inside your app, create two files:
* templatetags/__init__.py
* templatetags/blog_tags.py

Add template tag to templatetags/blog_tags.py

```python
from django.utils.safestring import mark_safe
import markdown

@register.filter(name='markdown')
def markdown_format(text):
    return mark_safe(markdown.markdown(text))
```

Add markdown filter to a template

```html
{% load blog_tags %}

{{ post.body|markdown }}
{{ post.body|markdown|truncatewords_html:30 }}
```

Add new post with this body:
```
This is a post formatted with markdown

-------------------------------------

*This is emphasized* and **this is more emphasized**.

 Here is a list:

 * One
 * Two
 * Three

 And a [link to the Django website](https://www.djangoproject.com/).
```
