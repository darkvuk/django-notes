# Sort Order

Objects will be returned in reverse chronological order by default.
```
class Meta:
    ordering = ['created']
```

Objects will be returned in chronological order by default.
```
class Meta:
    ordering = ['-created']
```
