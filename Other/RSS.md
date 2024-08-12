# RSS

Create app/feeds.py
```python
import markdown
from django.contrib.syndication.views import Feed
from django.template.defaultfilters import truncatewords_html
from django.urls import reverse_lazy
from .models import Post


class LatestPostsFeed(Feed):
    title = 'My blog'
    link = reverse_lazy('blog:post_list')
    description = 'New posts of my blog.'
    def items(self):
        return Post.published.all()[:5]
    def item_title(self, item):
        return item.title
    def item_description(self, item):
        return truncatewords_html(markdown.markdown(item.body), 30)
    def item_pubdate(self, item):
        return item.publish
```

Add this path to app/urls.py
```python
from .feeds import LatestPostsFeed

urlpatterns = [
    # ...
    path('feed/', LatestPostsFeed(), name='post_feed'),
]
```

RSS Viewer: https://github.com/yang991178/fluent-reader/releases

Add this link to a template
```html
<p>
   <a href="{% url "blog:post_feed" %}">
        Subscribe to my RSS feed
   </a>
</p>
```
