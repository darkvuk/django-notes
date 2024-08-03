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
