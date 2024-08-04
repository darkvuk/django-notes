The default manager for every model is the objects manager. This manager retrieves all the objects in the database. We can also define custom managers for models.

EXAMPLE: A custom manager to retrieve all posts that have a PUBLISHED status

```python
class PublishedManager(models.Manager):
    def get_queryset(self):
        return (
            super().get_queryset().filter(status=Post.Status.PUBLISHED)
        )
```

```python
class Post(models.Model):
    #...
    objects = models.Manager()
    published = PublishedManager()
```
