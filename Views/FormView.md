# FormView

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)

    def send_email(self):
        pass
```

```python
from .forms import ContactForm
from django.urls import reverse_lazy
from django.views.generic.edit import FormView


class ContactFormView(FormView):
    template_name = 'app/contact.html'
    form_class = ContactForm
    success_url = reverse_lazy('app:home')

    def get_initial(self):
        initial = super().get_initial()
        initial['name'] = 'Your Name'
        return initial

    def get_form_kwargs(self):
        kwargs = super().get_form_kwargs()
        # Add additional keyword arguments if needed
        kwargs['initial']['email'] = 'example@example.com'
        return kwargs

    def form_valid(self, form):
        # Custom logic when form is valid
        form.send_email()
        return super().form_valid(form)

    def form_invalid(self, form):
        # Custom logic when form is invalid
        print("Form is invalid")
        return super().form_invalid(form)
```
