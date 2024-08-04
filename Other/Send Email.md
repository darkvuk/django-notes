# Send Email

In app/forms.py add this code

```python
from django import forms


class EmailPostForm(forms.Form):
    name = forms.CharField(max_length=25)
    email = forms.EmailField()
    to = forms.EmailField()
    comment = forms.CharField(
        required=False,
        widget=forms.Textarea
    )
```

Add following view
```python
from .forms import EmailPostForm


def post_share(request, post_id):
    post = get_object_or_404(
        Post,
        id=post_id,
        status=Post.Status.PUBLISHED
    )
    sent = False

    if request.method == 'POST':
        form = EmailPostForm(request.POST)
        if form.is_valid():
            payload = form.cleaned_data
            
            post_url = request.build_absolute_uri(
                post.get_absolute_url()
            )

            subject = (
                f'{payload['name']} ({payload['email']}) '
                f'recommends you read \'{post.title}\''
            )

            message = (
                f'Read {post.title} at {post_url}\n\n'
                f'{payload['name']}\'s comment: \n{payload['comment']}'
            )

            send_mail(
                subject=subject,
                message=message,
                from_email=None,
                recipient_list=[payload['to']]
            )
            sent = True
        else:
            print(form.errors)
    else:
        form = EmailPostForm()
    
    return render(
        request,
        'blog/post/share.html',
        {
            'post': post,
            'form': form,
            'sent': sent
        }
    )
```

Install this package
```
pip install python-decouple
```

Add this to settings.py
```python
from decouple import config


EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER = config('EMAIL_HOST_USER')
EMAIL_HOST_PASSWORD = config('EMAIL_HOST_PASSWORD')
EMAIL_PORT = 587
EMAIL_USE_TLS = True
DEFAULT_FROM_EMAIL = config('DEFAULT_FROM_EMAIL')
```

Add this to .env file
```
EMAIL_HOST_USER=darko.vukasovic5@gmail.com
EMAIL_HOST_PASSWORD=****************
DEFAULT_FROM_EMAIL=My Blog <darko.vukasovic5@gmail.com>
```

Add this to app/urls.py
```python
path('<int:post_id>/share/', views.post_share, name='post_share'),
```

Create the template file
```html
{% extends "blog/base.html" %}

{% block title %}Share a post{% endblock %}

{% block content %}
 {% if sent %}
   <h1>E-mail successfully sent</h1>
   <p>
     "{{ post.title }}" was successfully sent to {{ form.cleaned_data.to }}.
   </p>
 {% else %}
   <h1>Share "{{ post.title }}" by e-mail</h1>
   <form method="post">
        {{ form.as_p }}
        {% csrf_token %}
        <input type="submit" value="Send e-mail">
    </form>
  {% endif %}
 {% endblock %}
```

Add this to detail template
```html
<p>
    <a href="{% url "blog:post_share" post.id %}">
        Share this post
    </a>
</p>
```
