# Google Authentication

```
pip install django-extensions werkzeug pyOpenSSL
```

Add to settings.py

```python
INSTALLED_APPS = [
   # ...
   'django_extensions',
]
```

```
python manage.py runserver_plus --cert-file cert.crt
```

Add to settings.py
```python
AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend',
    # ...
    'social_core.backends.google.GoogleOAuth2',
]
```

Go to https://console.cloud.google.com/projectcreate

![image](https://github.com/user-attachments/assets/dde2596f-c5bd-4e2a-beff-3198805bdfd5)
![image](https://github.com/user-attachments/assets/4a08f08d-387e-46bd-b1e6-f3634237655f)
![image](https://github.com/user-attachments/assets/8bc1c984-9570-4073-9956-050f34be574c)
![image](https://github.com/user-attachments/assets/0436f729-6b7a-4001-8a76-cf822df58fb0)
![image](https://github.com/user-attachments/assets/8b6197e8-d7ff-4e61-aed0-cca13239d6a1)
![image](https://github.com/user-attachments/assets/1fab68f5-8231-4fba-8025-285f16be6323)
![image](https://github.com/user-attachments/assets/31806ef6-0964-4d92-a6cc-3815427fa44c)
![image](https://github.com/user-attachments/assets/dcc74fe8-6753-49b7-9967-6f2203059e47)
![image](https://github.com/user-attachments/assets/af977290-5c61-4f92-b302-05a603c9843a)


Application type: `Web application`

Name: `Bookmarks`

Authorised JavaScript origins: `https://mysite.com:8000`

Authorised redirect URIs: `https://mysite.com:8000/social_authcomplete/google-oauth2/`

![image](https://github.com/user-attachments/assets/4adff384-8a6e-44d8-bed1-4315ae7d17d4)

.env
```env
GOOGLE_OAUTH2_KEY=xxxxxx
GOOGLE_OAUTH2_SECRET=xxxxxx
```

```
pip install python-decouple
```

Add this to settings.py
```python
from decouple import config

SOCIAL_AUTH_GOOGLE_OAUTH2_KEY = config('GOOGLE_OAUTH2_KEY')
SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET = config('GOOGLE_OAUTH2_SECRET')
```

Add this to login.html
```html
<div class="social">
    <ul>
        <li class="google">
            <a href="{% url "social:begin" "google-oauth2" %}">
                Sign in with Google
            </a>
        </li>
    </ul>
</div>
```

```
python manage.py runserver_plus --cert-file cert.crt
```
