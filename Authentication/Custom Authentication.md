# Custom Authentication

authentication.py

```python
from django.contrib.auth.models import User


class EmailAuthBackend:
    """Authenticate using email address."""

    def authenticate(sel, request, username=None, password=None):
        try:
            user = User.objects.get(email=username)
            if user.check_password(password):
                return user
            return None
        except(User.DoesNotExist, User.MultipleObjectsReturned):
            return None
        
    def get_user(self, user_id):
        try:
            return User.objects.get(pk=user_id)
        except User.DoesNotExist:
            return None
```

Add this list to settings.py. You can now authenticate using both username or email
```python
AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend',
    'account.authentication.EmailAuthBackend'
]
```
