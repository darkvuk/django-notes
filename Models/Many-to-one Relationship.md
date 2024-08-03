# Many-to-one Relationship / Foreign Key

Relationship with user

```python
 from django.conf import settings
```

```python
author = models.ForeignKey(
    to=settings.AUTH_USER_MODEL,
    on_delete=models.CASCADE,
    related_name='blog_posts'
)
```
`to` - the model that the foreign key is related to

`on_delete`
* `models.CASCADE` - when the referenced object is deleted, also delete the objects that have foreign keys to it
* `models.PROTECT` - prevent deletion of the referenced object if there are any objects pointing to it
* `models.SET_NULL` - set the foreign key to NULL when the referenced object is deleted

`related_name` - the name to use for the reverse relation
