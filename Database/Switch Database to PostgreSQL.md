# Switch Database to PostgreSQL

Install Docker

```
docker pull postgres:16.2
```

```
docker run --name=blog_db -e POSRGRES_DB=blog -e POSTGRES_USER=blog -e POSTGRES_PASSWORD=xxxxx -p 5432:5432 -d postgres:16.2
```

```
pip install psycopg2
```

```
python manage.py dumpdata --indent=2 --output=mysite_data.json --exclude contenttypes
```

```
pip install python-decouple
```

Next, you need to change the database in settings.py
```python
from decouple import config

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USER'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST')
    }
}
```

Add these values to .env
```env
DB_NAME=blog
DB_USER=blog
DB_PASSWORD=xxxxx
DB_HOST=localhost
```

```
python manage.py migrate
```

```
python manage.py loaddata mysite_data.json
```

Add to installed files in settings.py
```python
INSTALLED_APPS = [
    # ...
    'django.contrib.postgres'
]
```
