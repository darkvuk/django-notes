# Models

You write them in app/models.py

Import following lines
```python
from django.db import models
```

I usually write models in this order

```python
class ModelName(models.Model):
    # fields
    # def __str__(self):
```

Here's an example
```python
class Post(models.Model):
    title = models.CharField(max_length=250)
    body = models.TextField()
    
    def __str__(self):
        return self.title
```
