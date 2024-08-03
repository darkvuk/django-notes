# Django Admin

You need to register each model in admin.py in order to display it in Django admin.

<hr>

Without customization:
```python
from .models import Post

admin.site.register(Post)
```

<hr>

With customization:
```python
from django.contrib import admin
from .models import Post
```

```python
@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ['title', 'slug', 'author', 'publish', 'status']
    list_filter = ['status', 'created', 'publish', 'author']
    search_fields = ['title', 'body']
    prepopulated_fields = {'slug': ['title']}
    raw_id_fields = ['author']
    date_hierarchy = 'publish'
    ordering = ['status', 'publish']
    show_facets = admin.ShowFacets.ALWAYS
```

`list_display` -  set the fields of your model that you want to display on the list page

`list_filter` -  right sidebar that allows you to filter the results by the fields

`raw_id_fields` - a lookup widget, you can type id or open new window with all the objects of a specific class

`search_fields` - a list of searchable fields using the search field

`show_facets` -  right sidebar filters include total facet counts
`date_hierarchy` -  navigation links to navigate through a date hierarchy

`ordering` - specifies the default sorting criteria

`prepopulated_fields` - prepopulate the slug field with the input of the title field 
