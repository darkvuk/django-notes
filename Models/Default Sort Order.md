# Sort Order

Objects will be returned in reverse chronological order by default.

```python
class Meta:
    ordering = ['-created']
    indexes = [
        models.Index(fields=['-created']),
    ]
```

Objects will be returned in chronological order by default.
```python
class Meta:
    ordering = ['created']
    indexes = [
        models.Index(fields=['created']),
    ]
```

