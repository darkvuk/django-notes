# Registration

forms.py
```python
from django import forms
from django.contrib.auth import get_user_model
from .models import Profile


class UserRegistrationForm(forms.ModelForm):
    password = forms.CharField(
        label='Password',
        widget=forms.PasswordInput
    )
    password2 = forms.CharField(
        label='Repeat password',
        widget=forms.PasswordInput
    )

    class Meta:
        model = get_user_model()
        fields = ['username', 'first_name', 'last_name']

    # method is executed when form is validated by calling is_valid()
    def clean_password2(self):
        cd = self.cleaned_data
        if cd['password'] != cd['password2']:
            raise forms.ValidationError("Passwords don't match.")
        return cd['password2']
```

views.py
```python
from django.shortcuts import render
from .forms import UserRegistrationForm
from .models import Profile


def register(request):
    if request.method == 'POST':
        user_form = UserRegistrationForm(request.POST)
        if user_form.is_valid():
            new_user = user_form.save(commit=False)
            new_user.set_password(
                user_form.cleaned_data['password']
            )
            new_user.save()
            Profile.objects.create(user=new_user)
            return render(
                request,
                'account/register_done.html',
                {'new_user': new_user}
            )
    else:
        user_form = UserRegistrationForm()
    return render(
        request,
        'account/register.html',
        {'user_form': user_form}
    )
```

urls.py
```python
path('register/', views.register, name='register'),
```

app/templates/account/register.html
```
{% extends 'base.html' %}

{% block title %}
Create an account
{% endblock %}

{% block content %}
    <h1>Create an account.</h1>
    <form method="post">
        {{ user_form.as_p }}
        {% csrf_token %}
        <p><input type="submit" value="Create account"></p>
    </form>
{% endblock %}
```

app/templates/account/registe_done.html
```
{% extends 'base.html' %}

{% block title %}
Welcome
{% endblock %}

{% block content %}
    <h1>Welcome, {{ new_user.first_name }}!</h1>
    <p>
        Your account has been successfully created.
        Now you can <a href="{% url "login" %}">log in</a>.
    </p>
{% endblock %}
```
