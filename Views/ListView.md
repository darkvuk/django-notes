# ListView

```python
path('list/', PostListView.as_view(), name='post_list')
```

```python
from django.http import Http404
from django.core.paginator import EmptyPage, PageNotAnInteger, Paginator
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from .models import Post
from django.urls import reverse_lazy
from .forms import PostForm


class PostListView(ListView):
    model=Post
    template_name = 'app/list.html'
    context_object_name='posts'
    paginate_by = 1
    page_kwarg = 'page'
    # ordering = ['-created_at']

    def get_queryset(self):
        return Post.objects.filter(active=True).order_by('-created_at')
    
    def get_template_names(self):
        # Custom template names logic
        return [self.template_name]
    
    def get_allow_empty(self):
        # Allow the list to be empty - DEFAULT
        return True  

    def paginate_queryset(self, queryset, page_size):
        paginator = self.get_paginator(queryset, page_size)
        page_kwarg = self.page_kwarg
        page = self.kwargs.get(page_kwarg) or self.request.GET.get(page_kwarg) or 1

        try:
            page_number = paginator.validate_number(page)
            page = paginator.page(page_number)
            return (paginator, page, page.object_list, page.has_other_pages())
        except (PageNotAnInteger, EmptyPage):
            page_number = 1
            page = paginator.page(page_number)
            return (paginator, page, page.object_list, page.has_other_pages())
```

```html
{% for post in posts %}
  <h1>{{ post.title }}</h1>
  <p>{{ post.content }}</p>
  <p>{{ post.author }}</p>
{% endfor %}

{% if is_paginated %}
  <div class="pagination">
    <span class="step-links">
      {% if page_obj.has_previous %}
        <a href="?page=1">&laquo; first</a>
        <a href="?page={{ page_obj.previous_page_number }}">previous</a>
      {% endif %}

      <span class="current">
        Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}.
      </span>

      {% if page_obj.has_next %}
        <a href="?page={{ page_obj.next_page_number }}">next</a>
        <a href="?page={{ page_obj.paginator.num_pages }}">last &raquo;</a>
      {% endif %}
    </span>
  </div>
{% endif %}
```
