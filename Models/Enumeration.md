# Enumeration

```python
class Post(models.Model):

    class Status(models.TextChoices):
        DRAFT = 'DF', 'Draft'
        PUBLISHED = 'PB', 'Published'

    #...

    status = models.CharField(
        max_length=2,
        choices=Status,
        default=Status.DRAFT
    )
```
