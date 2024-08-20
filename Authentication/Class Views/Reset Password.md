```python
from django.contrib.auth import views as auth_views
from django.urls import path

urlpatterns = [
    
    # reset password urls
    path(
        'password-reset/',
        auth_views.PasswordResetView.as_view(),
        name='password_reset'
    ),
    path(
        'password-reset/done/',
        auth_views.PasswordResetView.as_view(),
        name='password_reset_done'
    ),
    path(
        'password-reset/<uidb64>/<token>/',
        auth_views.PasswordResetConfirmView.as_view(),
        name='password_reset_confirm'
    ),
    path('password-reset/complete/',
        auth_views.PasswordResetCompleteView.as_view(),
        name='password_reset_complete'
    ),
]
```

app\templates\registration\password_reset_form.html
```
{% extends "base.html" %}

{% block title %}Reset your password{% endblock %}

{% block content %}
    <h1>Forgotten your password?</h1>
    <p>Enter your e-mail address to obtain a new password.</p>

    <form method="post">
        {{ form.as_p }}
        <p><input type="submit" value="Send e-mail"></p>
        {% csrf_token %}
    </form>
{% endblock %}
```

app\templates\registration\password_reset_email.html
```
Someone asked for password reset for email {{ email }}. Follow the link below:

{{ protocol }}://{{ domain }}{% url "password_reset_confirm" uidb64=uid 
token=token %}

Your username is: {{ user.get_username }}
```

app\templates\registration\password_reset_done.html
```
{% extends "base.html" %}

{% block title %}
Reset your password
{% endblock %}

{% block content %}
    <h1>Reset your password</h1>
    <p>We've emailed you instructions for setting your password.</p>
    <p>
        If you don't receive an email, please make sure you've entered the address you registered with.
    </p>
{% endblock %}
```

app\templates\registration\password_reset_confirm.html
```
{% extends "base.html" %}

{% block title %}
Reset your password
{% endblock %}


{% block content %}
    <h1>Reset your password</h1>
    
    {% if validlink %}
        <p>Please enter your new password twice:</p>
        <form method="post">
            {{ form.as_p }}
            {% csrf_token %}
            <p><input type="submit" value="Change my password" /></p>
        </form>
    {% else %}
        <p>
            The password reset link was invalid, possibly because it has already been used. 
            Please request a new password reset.
        </p>
    {% endif %}
{% endblock %}
```

Add password reset link to login page
```
<p>
    <a href="{% url "password_reset" %}">
        Forgotten your password?
    </a>
</p>
```

For testing, add this to settings.py. The email will be printed on your console.
```python
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```

Email exmaple
```
Content-Type: text/plain; charset="utf-8"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Subject: Password reset on 127.0.0.1:8000
From: webmaster@localhost
To: test@gmail.com
Date: Mon, 10 Jan 2024 19:05:18 -0000
Message-ID: <162896791878.58862.14771487060402279558@MBP-amele.local>
Someone asked for password reset for email test@gmail.com. Follow the link 
below:
http://127.0.0.1:8000/account/password-reset/MQ/ardx0u
b4973cfa2c70d652a190e79054bc479a/
Your username, in case you've forgotten: test
```

