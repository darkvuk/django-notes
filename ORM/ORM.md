# Object-Relational Mapper (ORM) and Shell

```
python manage.py shell
```

Import the models you want to work with
```python
from django.contrib.auth.models import User
from blog.models import Post
```

* `get` - retrieve a single user object
```python
User.objects.get()
```

* SELECT * FROM User WHERE username='darko' (retrieve a single user object and save the result in a variable)
```python
user = User.objects.get(username='darko')
```

* `all()` - get all the objects - SELECT * FROM Post
```python
Post.objects.all()
```

* `filter()` - filter objects - SELECT * FROM Post WHERE title='Another Post'
```python
Post.objects.filter(title='Another post')
```

* `values()` - get specific fields from objects - SELECT title, slug FROM Post
```python
posts = Post.objects.values('title', 'slug')
```

* Print the SQL query
```python
print(posts.query)
```

* Insert object into table
```python
post = Post(title='Another post 2', slug='another-post2', body='Post body.', author=user)
post.save()
```

* `get_or_create` - if an object doesn't exist, create it
```python
user, created = User.objects.get_or_create(username='random_user')
```



* Update object
```python
post.title = 'New title'
post.save()
```
