# django.contrib.auth.urls

```python
from django.contrib.auth import views as auth_views
from django.urls import include, path
from . import views

urlpatterns = [
    path('', include('django.contrib.auth.urls')),
    path('', views.dashboard, name='dashboard'),
]
```

![image](https://github.com/user-attachments/assets/7f1a45f8-6148-4000-a2d1-1ff444da6c13)

