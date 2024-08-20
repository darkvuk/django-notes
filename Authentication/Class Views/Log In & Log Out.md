These are the URLs

```python
from django.contrib.auth.views import (
    LoginView,
    LogoutView
)
from django.urls import path
from . import views

urlpatterns = [
    path('login/', LoginView.as_view(), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('', views.dashboard, name='dashboard'),
]
```

Add URLs to settings.py. Make sure to put User app as a first app in INSTALLED_APPS
```python
LOGIN_REDIRECT_URL = 'dashboard'
LOGIN_URL = 'login'
LOGOUT_URL = 'logout'
```

Login form
app/templates/registration/login.html
```
{% extends 'base.html' %}

{% block title %}
Log In
{% endblock %}

{% block content %}
    <h1>Log In</h1>
    {% if form.errors %}
        <p>Your username and password didn't match.</p>
    {% endif %}
    <form action="{% url 'login' %}" method="post">
        {{ form.as_p }}
        {% csrf_token %}
        <input type="hidden" name="next" value="{{ next }}" />
        <p><input type="submit" value="Log in"></p>
    </form>
{% endblock %}
```

Logout form (put it somewhere in a header)
```
<form action="{% url "logout" %}" method="post">
    <button type="submit">Logout</button>
    {% csrf_token %}
</form>
```

This link will be displayed after you click Logout.

app/templates/registration/logged_out.html
```
{% extends "base.html" %}

{% block title %}
Logged out
{% endblock %}

{% block content %}
    <h1>Logged out</h1>
    <p>
        You have been successfully logged out.
        You can <a href="{% url "login" %}">log-in again</a>.
    </p>
{% endblock %}
```

```python
from django.contrib.auth.decorators import login_required
from django.shortcuts import render


@login_required
def dashboard(request):
    return render(
        request,
        'account/dashboard.html',
        {'section': 'dashboard'}
    )
```
