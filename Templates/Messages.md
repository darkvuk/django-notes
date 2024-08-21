# Messages

Add to base.html
```html
{% if messages %}
    <ul class="messages">
        {% for message in messages %}
            <li class="{{ message.tags }}">
                {{ message | safe }}
                <a href="#" class="close">x</a>
            </li>
        {% endfor %}
    </ul>
{% endif %}
```

views.py
```python
if user_form.is_valid() and profile_form.is_valid():
    user_form.save()
    profile_form.save()
    messages.success(
        request,
        'Profile updated successfully.'
    )
else:
    messages.error(request, 'Error updating your profile.')
```
