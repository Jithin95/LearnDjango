# Html Forms

> form.html

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
    <head>
        <meta charset="utf-8">
        <title>Index</title>
    </head>
    <body>
        <h1>{{val}}</h1>
        <form action="{% url 'contact_index' %}" method="post">
            {% csrf_token %}
            <label for="name">Your Name:</label>
            <input type="text" name="username" id="name"/>
            <input type="submit" value="Submit"/>
        </form>
    </body>
</html>
```

> views.py

```python
def index(request):
    if request.method == 'POST':
        return render(request, 'contact/index.html', {'val':request.POST['username']})
    else:
        return render(request, 'contact/index.html', {'val':'notget'})
```

# Django Forms

> forms.py

```python
from django import forms

class NameForm(forms.Form):
    yourName = forms.CharField(label = "Your Name:", max_length=100, required = False)

```

> views.py

```python

class NameView(TemplateView):
    template_name = 'contact/nameform.html'

    # When GET Request
    def get(self, request):
        form = NameForm()
        return render(request, self.template_name, {'form': form})

    # When POST Request
    def post(self, request):
        form = NameForm(request.POST)
        if form.is_valid():
            yourname = form.cleaned_data['yourName']
            
            # redirect to a new URL:
            return HttpResponseRedirect('/thanks/')
        else: # when some error on the field including custom
            return render(request, self.template_name, {'form': form})

```

> urls.py

```python
from django.urls import path
from . import views
urlpatterns = [
    path('', views.NameView.as_view(), name= 'nameview'),
]
```

> templates/contact/nameform.html

```html
<body>
    <form action="{% url 'nameview' %}" method="post">
        {% csrf_token %}
        {{form}}
        <input type="submit" value="Submit"/>
    </form>
</body>

```
