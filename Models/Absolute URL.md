# Absolute URL

```python
from django.urls import reverse
```

```python
def get_absolute_url(self):
    return reverse(
        'blog:post_detail',
        args=[self.id]
    )
```

Instead of
```html
<a href="{% url 'blog:post_detail' post.id %}">
```
Use
```html
<a href="{{ post.get_absolute_url }}">
```
