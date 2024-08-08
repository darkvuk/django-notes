# Django Taggit

```
pip install django-taggit
```

```python
INSTALLED_APPS = [
    # ...
    'taggit'
 ]
```

In models.py add
```python
from taggit.managers import TaggableManager

class Post(models.Model):
    # ...
    tags = TaggableManager()
```

```python
python manage.py makemigrations; python manage.py migrate
```

<hr>

### Add tags via shell

```
python manage.py shell
```

```python
from blog.models import Post
post = Post.objects.get(id=1)
post.tags.add('music', 'jazz', 'django')
# post.tags.remove('django')
```

### Display posts by tags

In list view in views.py, after the initial query, add this
```python
tag = None

if tag_slug:
    tag = get_object_or_404(Tag, slug=tag_slug)
    post_list = post_list.filter(tags__in=[tag])

# Add this to context 'tag': tag
```

Add to url.py

```python
path(
 'tag/<slug:tag_slug>/', views.post_list, name='post_list_by_tag'
    ),
```

Add this to list template
```html
{% if tag %}
    <h2>Posts tagged with "{{ tag.name }}"</h2>
{% endif %}
```

For each post in the list display its tags
```
<p class="tags">
    Tags:
    {% for tag in post.tags.all %}
        <a href="{% url "blog:post_list_by_tag" tag.slug %}">
          {{ tag.name }}
        </a>{% if not forloop.last %}, {% endif %}
    {% endfor %}
</p>
```

