# Creating app

```powershell
python manage.py startapp blog
```
`settings.py`
```python
 INSTALLED_APPS = [
    #...
    'blog.apps.BlogConfig',
 ]
```
You need to register each model from the app in `admin.py`
```python
from .models import Post

admin.site.register(Post)
```
