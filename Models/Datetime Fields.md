# Datetime Fields

Import 
```python
 from django.utils import timezone
```

An example of datetime field
```python
datetime = models.DateTimeField(default=timezone.now)
```

Adding created and updated fields
```python
created = models.DateTimeField(auto_now_add=True)
updated = models.DateTimeField(auto_now=True)
```

`auto_now_add` -  the date will be saved automatically when creating an object.
`auto_now` -  the date will be updated automatically when saving an object.
