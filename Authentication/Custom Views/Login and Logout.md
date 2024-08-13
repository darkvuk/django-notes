# Log in

```
django-admin startapp account
```

Add it to settings.py

```python
INSTALLED_APPS = [
    'account.apps.AccountConfig',
    # ...
]
```

```
python manage.py migrate
```

Create forms.py

```python
from django import forms

class LoginForm(forms.Form):
    username = forms.CharField()
    password = forms.CharField(widget=forms.PasswordInput)
```

Add these views to views.py

```python
from django.contrib.auth import authenticate, login, logout
from django.http import HttpResponse
from django.shortcuts import render, redirect
from .forms import LoginForm
```

```python
def user_login(request):
    if request.method == 'POST':
        form = LoginForm(request.POST)
        if form.is_valid():
            payload = form.cleaned_data
            user = authenticate(
                request,
                username=payload['username'],
                password=payload['password']
            )
            if user is not None:
                if user.is_active:
                    login(request, user)
                    return HttpResponse('Authenticated successfully.')
                else:
                    return HttpResponse('Disabled account.')
            else:
                return HttpResponse('Invalid login')
    else:
        form = LoginForm()
    
    return render(request, 'account/login.html', {'form': form})
```

```python
def user_logout(request):
    logout(request)
    return redirect('login')
```

Create urls

```python
from django.urls import path
from . import views

urlpatterns = [
    path('login/', views.user_login, name='login'),
    path('logout/', views.user_logout, name='logout')
]
```
