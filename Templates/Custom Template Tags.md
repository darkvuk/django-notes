# Custom Template Tags

## simple_tag

simple_tag is a type of custom template tag in Django that allows you to write a simple Python function and make it available in your templates. This tag is ideal when you want to perform some processing and return a result that can be directly used in the template.

Inside your app, create two files:
* templatetags/__init__.py
* templatetags/blog_tags.py

Add template tag to templatetags/blog_tags.py
```python
from django import template
from ..models import Post

register = template.Library()

@register.simple_tag
def total_posts():
    return Post.published.count()
```

Load it in a template
```html
{% load blog_tags %}
<p>I've written {% total_posts %} posts so far.</p>
```
