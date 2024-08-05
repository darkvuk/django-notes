# TemplateView

Function based view

```python
urlpatterns = [
    path('', views.home, name='home')
]
```

```python
def home(request):
    return render(request, 'app/home.html')
```

Class based view

```python
from .views import Home


urlpatterns = [
    path('', Home.as_view(), name='home')
]
```

```python
class Home(TemplateView):
    template_name = 'app/home.html'
```

If you want to pass additional context to a template

```python
class Home(TemplateView):
    template_name = 'app/home.html'

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['welcome_message'] = 'Welcome!'
        return context
```

Switch between templates

```python
class Home(TemplateView):

    def get_template_names(self):
        condition = True

        if condition:
            return ['app/page1.html']
        return ['app/page2.html']
```

Accepted HTTP methods

```python
class Home(TemplateView):
    template_name = 'app/page1.html'
    http_method_names = ['get', 'post']
```

Combination of different methods

```python
from django.shortcuts import render
from django.views.generic import TemplateView

class CustomTemplateView(TemplateView):
    template_name = 'home.html'
    http_method_names = ['get', 'post']  # Only accept GET and POST requests

    def get(self, request, *args, **kwargs):
        # Custom GET logic
        return self.render_to_response(self.get_context_data())

    def post(self, request, *args, **kwargs):
        # Custom POST logic
        context = self.get_context_data()
        context['post_data'] = request.POST
        return self.render_to_response(context)

    def dispatch(self, request, *args, **kwargs):
        # Custom logic for all requests
        print('Dispatching request')
        return super().dispatch(request, *args, **kwargs)

    def get_template_names(self):
        if 'alternative' in self.request.GET:
            return ['alternative_template.html']
        return ['home.html']

    def render_to_response(self, context, **response_kwargs):
        # Custom rendering logic
        return render(self.request, self.get_template_names(), context)

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['extra_data'] = 'This is extra context data'
        return context
```
