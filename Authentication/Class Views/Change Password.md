# Change Password

```
from django.contrib.auth.views import (
    LoginView,
    LogoutView,
    PasswordChangeView,
    PasswordChangeDoneView
)
from django.urls import path

urlpatterns = [
    path(
        'password-change/',
        PasswordChangeView.as_view(),
        name='password_change'     
    ),
    path(
        'password-change/done/',
        PasswordChangeDoneView.as_view(),
        name='password_change_done'
    )
]
```

app\templates\registration\password_change_form.html
```
{% extends 'base.html' %}

{% block title %}
Change your password
{% endblock %}

{% block content %}
    <h1>Chnage your password</h1>
    <form method="post">
        {{ form.as_p }}
        <p><input type="submit" value="Chnage"></p>
        {% csrf_token %}
    </form>
{% endblock %}
```

app\templates\registration\password_change_done.html
```
{% extends "base.html" %}

{% block title %}Password changed{% endblock %}

{% block content %}
    <h1>Password changed</h1>
    <p>Your password has been successfully changed.</p>
{% endblock %}
```
