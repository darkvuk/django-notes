```python
from django.shortcuts import render, redirect
from django.urls import reverse_lazy
from .forms import PostForm  # Assuming you have a form for the Post model
from .models import Post

def post_create_view(request):
    if request.method == 'POST':
        form = PostForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect(reverse_lazy('post_list'))
    else:
        form = PostForm()
    
    return render(request, 'post_form.html', {'form': form})
```
