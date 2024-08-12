# inclusion_tag

An inclusion_tag is a type of custom template tag that is used to render a specific template fragment with its own context. 

Inside your app, create two files:
* templatetags/__init__.py
* templatetags/blog_tags.py

Add template tag to templatetags/blog_tags.py

```python
from django import template
from ..models import Post

register = template.Library()

@register.inclusion_tag('blog/post/latest_posts.html')
def show_latest_posts(count=5):
    latest_posts = Post.published.order_by('-publish')[:count]
    return {'latest_posts': latest_posts}
```

The content of blog/post/latest_posts.html
```python
<ul>
    {% for post in latest_posts %}
        <li>
            <a href="{{ post.get_absolute_url }}">{{ post.title }}</a>
        </li>
    {% endfor %}
</ul>
```

Add this fragment to another template
```python
{% load blog_tags %}
<h3>Latest posts</h3>
{% show_latest_posts 3 %}
```
