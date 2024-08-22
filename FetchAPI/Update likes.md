# Update likes using Fetch API

The JavaScript Fetch API is the built-in way to make asynchronous HTTP requests to web servers 
from web browsers. By using the Fetch API, you can send and retrieve data from the web server 
without the need for a whole page refresh.

urls.py
```python
path('like/', views.image_like, name='like'),
```

views.py
```python
from django.contrib.auth.decorators import login_required
from django.http import JsonResponse
from django.views.decorators.http import require_POST
from .models import Image


@login_required
@require_POST
def image_like(request):
    image_id = request.POST.get('id')
    action = request.POST.get('action')
    
    if image_id and action:
        try:
            image = Image.objects.get(id=image_id)
            if action == 'like':
                image.users_like.add(request.user)
            elif action == 'unlike':
                image.users_like.remove(request.user)
            return JsonResponse({'status': 'ok'})
        except Image.DoesNotExist:
            pass
    return JsonResponse({'status': 'error'})
```
base.html
```html
<script src="//cdn.jsdelivr.net/npm/js-cookie@3.0.5/dist/js.cookie.min.js"></script>
    <script>
        const csrftoken = Cookies.get('csrftoken');
        document.addEventListener('DOMContentLoaded', (event) => {
            // DOM loaded
            {% block domready %}
            {% endblock %}
        })
</script>
```

detail.html
```js
{% block domready %}
    const url = '{% url "images:like" %}';
    var options = {
        method: 'POST',
        headers: {'X-CSRFToken': csrftoken},
        mode: 'same-origin'
    };

    document.querySelector('a.like')
        .addEventListener('click', function(e) {

        // Prevent default behavior of the anchor element
        e.preventDefault();
        var likeButton = this;

        // Add request body
        var formData = new FormData();
        formData.append('id', likeButton.dataset.id);
        formData.append('action', likeButton.dataset.action);
        options['body'] = formData;

        // Send HTTP request
        fetch(url, options)
            .then(response => response.json())
            .then(data => {
                if (data['status'] === 'ok') {
                    var previousAction = likeButton.dataset.action;
                    var action = previousAction === 'like' ? 'unlike' : 'like';
                    likeButton.dataset.action = action;
                    likeButton.innerHTML = action;

                    var likeCount = document.querySelector('span.count .total');
                    var totalLikes = parseInt(likeCount.innerHTML);
                    likeCount.innerHTML = previousAction === 'like' ? totalLikes + 1 : totalLikes - 1;
                }
            });
    });
{% endblock %}
```
