# Slugs for SEO Friendly URLs

```python
slug = models.SlugField(
    max_length=250,
    unique_for_date='publish'
)
```

In app/urls.py replace
```python
path('<int:id>/', views.post_detail, name='post_detail')
```
with
```python
path(
    '<int:year>/<int:month>/<int:day>/<slug:post>/',
    views.post_detail,
    name='post_detail'
)
```

Change detail view
```python
 def post_detail(request, year, month, day, post):
    post = get_object_or_404(
        Post,
        status=Post.Status.PUBLISHED,
        slug=post,
        publish__year=year,
        publish__month=month,
        publish__day=day
    )

    return render(
        request,
        'blog/post/detail.html',
        {'post': post}
    )
```

Modify absolute URL method
```python
def get_absolute_url(self):
    return reverse(
        'blog:post_detail',
        args=[
            self.publish.year,
            self.publish.month,
            self.publish.day,
            self.slug
        ]
    )
```
