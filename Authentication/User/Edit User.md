# Edit User

```python
from django import forms
from django.contrib.auth import get_user_model
from .models import Profile


class UserEditForm(forms.ModelForm):
    class Meta:
        model = get_user_model()
        fields = ['first_name', 'last_name', 'email']


class ProfileEditForm(forms.ModelForm):
    class Meta:
        model = Profile
        fields = ['date_of_birth', 'photo']
```

views.py
```python
from django.contrib.auth.decorators import login_required
from django.shortcuts import render
from .forms import (
    UserEditForm,
    ProfileEditForm
)


@login_required
def edit(request):
    if request.method == 'POST':
        user_form = UserEditForm(
            instance=request.user,
            data=request.POST
        )
        profile_form = ProfileEditForm(
            instance=request.user.profile,
            data=request.POST,
            files=request.FILES
        )
        if user_form.is_valid() and profile_form.is_valid():
            user_form.save()
            profile_form.save()
    else:
        user_form = UserEditForm(instance=request.user)
        profile_form = ProfileEditForm(instance=request.user.profile)
    return render(
        request,
        'account/edit.html',
        {
            'user_form': user_form,
            'profile_form': profile_form
        }
    )
```

urls.py
```
path('edit/', views.edit, name='edit')
```

edit.html
```
{% extends 'base.html' %}

{% block title %}
Edit account
{% endblock %}

{% block content %}
    <h1>Edit your account</h1>
    <form method="post" enctype="multipart/form-data">
        {{ user_form.as_p }}
        {{ profile_form.as_p }}
        {% csrf_token %}
        <p><input type="submit" value="Save changes"></p>
    </form>
{% endblock %}
```
