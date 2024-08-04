# Pagination

Modify list view
```python
from django.core.paginator import Paginator

def post_list(request):
    post_list = Post.published.all()

    paginator = Paginator(post_list, 3)
    page_number = request.GET.get('page', 1)

    try:
        posts = paginator.page(page_number)
    except PageNotAnInteger:
        posts = paginator.page(1)
    except EmptyPage:
        posts = paginator.page(paginator.num_pages)

    return render(
        request,
        'blog/post/list.html',
        {'posts': posts}
    )
```

Create template pagination.html

```html
<div class="pagination">
    <span class="step-links">
      {% if page.has_previous %}
        <a href="?page={{ page.previous_page_number }}">Previous</a>
      {% endif %}
      <span class="current">
        Page {{ page.number }} of {{ page.paginator.num_pages }}.
      </span>
      {% if page.has_next %}
        <a href="?page={{ page.next_page_number }}">Next</a>
      {% endif %}
    </span>
</div>
```

At the bottom of list template add this line

```html
{% include "pagination.html" with page=posts %}
```
