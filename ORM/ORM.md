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

* Delete object
```python
post = Post.objects.get(id=1)
post.delete()
```

* `exclude()`
```python
Post.objects.exclude(title__startswith='Why')
```

* `count()`
```python
Post.objects.filter(id_lt=3).count()
```

* `exists()`
```python
Post.objects.filter(id=3).exists()
```

<hr>
<br>

# Field Lookups

`__exact` - These two queries search for the objects with the exact id
```python
Post.objects.filter(id__exact=1)
```
```python
Post.objects.filter(id=1)
```

`__iexact` - Case-insensitive lookup
```python
Post.objects.filter(title__iexact='another post')
```

`__contains` - Equivalent of LIKE in SQL
```python
Post.objects.filter(title__contains='Post')
```

`__icontains` - Case-insensitive version
```python
Post.objects.filter(title__icontains='Post')
```

`__in` - OR
```python
Post.objects.filter(id__in=[1, 5]).values('id')
```

* `__gt`  -  >
* `__gte`  -  >=
* `__lt`  -  <
* `__lte`  -  <=
```python
Post.objects.filter(id__gt=3)
```

* `__startswith`
* `__istartswith`
* `__endswith`
* `__iendswith`
```python
Post.objects.filter(title__istartswith='who')
```

`__date` - Date lookup
```python
from datetime import date
Post.objects.filter(created__date=date(2024,8,3))
```

* `__year`
* `__month`
* `__day`
```python
from datetime import date
Post.objects.filter(publish__year=2024)
```


* `__date__gt`
* `__date__gte`
* `__date__lt`
* `__date__lte` 
```python
from datetime import date
Post.objects.filter(created__date__gt=date(2024,7,23))
```

Lookup related object fields
```python
Post.objects.filter(author__username='darko')
```

Chain additional lookups
```python
Post.objects.filter(author__username__istartswith='da')
```

<hr>
<br>

# Ordering objects
A->Z
```python
Post.objects.order_by('title')
```

Z->A
```python
Post.objects.order_by('-title')
```

Order by multiple fields
```python
Post.objects.order_by('author', 'title')
```

Order randomly
```python
Post.objects.order_by('?')
```

<hr>
<br>

# Limit Querysets
Return third and fourth object
```python
Post.objects.all()[2:4]
```

Return random object
```python
Post.objects.order_by('?')[0]
```
