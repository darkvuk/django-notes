# DetailView

```python
from django.views.generic import ListView, DetailView
from .models import Post
```

```python
class PostDetailView(DetailView):
    model = Post
    template_name = 'app/detail.html'
    context_object_name = 'post'
```
