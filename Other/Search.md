# Search

Make sure you use PostgreSQL for your project.

Add this form to forms.py
```python
class SearchForm(forms.Form):
    query = forms.CharField()
```

In views.py, add this

```python
from django.contrib.postgres.search import SearchVector
from .forms import SearchForm


def post_search(request):
    form = SearchForm()
    query = None
    results = []

    if 'query' in request.GET:
        form = SearchForm(request.GET)
        if form.is_valid():
            query = form.cleaned_data['query']
            results = (
                Post.published
                .annotate(search=SearchVector('title', 'body'))
                .filter(search=query)
            )

    return render(
        request,
        'blog/post/search.html',
        {
            'form': form,
            'query': query,
            'results': results
        }
    )
```

Create a new template called search.html
```html
{% extends "blog/base.html" %}
{% load blog_tags %}

{% block title %}Search{% endblock %}

{% block content %}
    {% if query %}
        <h1>Posts containing "{{ query }}"</h1>
        <h3>
            {% with results.count as total_results %}
                Found {{ total_results }} result{{ total_results|pluralize }}
            {% endwith %}
        </h3>
        {% for post in results %}
            <h4>
                <a href="{{ post.get_absolute_url }}">
                    {{ post.title }}
                </a>
            </h4>
            {{ post.body|markdown|truncatewords_html:12 }}
        {% empty %}
            <p>There are no results for your query.</p>
        {% endfor %}
        <p><a href="{% url "blog:post_search" %}">Search again</a></p>
    {% else %}
        <h1>Search for posts</h1>
        <form method="get">
            {{ form.as_p }}
            <input type="submit" value="Search">
        </form>
    {% endif %}
{% endblock %}
```

Add this path to app/urls.py

```python
path('search/', views.post_search, name='post_search'),
```

<hr>

## Ranking results

```python
from django.contrib.postgres.search import (
    SearchVector,
    SearchQuery,
    SearchRank
)
```

```python
def post_search(request):
    form = SearchForm()
    query = None
    results = []

    if 'query' in request.GET:
        form = SearchForm(request.GET)
        if form.is_valid():
            query = form.cleaned_data['query']
            search_vector = SearchVector('title', 'body')
            search_query = SearchQuery(query)
            results = (
                Post.published.annotate(
                    search=search_vector,
                    rank=SearchRank(search_vector, search_query)
                )
                .filter(search=search_query)
                .order_by('-rank')
            )

    return render(
        request,
        'blog/post/search.html',
        {
            'form': form,
            'query': query,
            'results': results
        }
    )
```

<hr>

##  Stemming and removing stop words in different languages

```python
search_vector = SearchVector('title', 'body', config='spanish')
search_query = SearchQuery(query, config='spanish')
```

<hr>

## Weighting queries

```python
 def post_search(request):
    form = SearchForm()
    query = None
    results = []

    if 'query' in request.GET:
        form = SearchForm(request.GET)
        if form.is_valid():
                    query = form.cleaned_data['query']
                    search_vector = SearchVector(
                       'title', weight='A'
                    ) + SearchVector('body', weight='B')
                    search_query = SearchQuery(query)
                    results = (
                        Post.published.annotate(
                            search=search_vector,
                            rank=SearchRank(search_vector, search_query)
                        )
                        .filter(rank__gte=0.3)
                        .order_by('-rank')
                    )

    return render(
        request,
       'blog/post/search.html',
        {
            'form': form,
            'query': query,
            'results': results
        }
    )

```
