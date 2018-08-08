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

> Output

```html
<form action="/contact/" method="post">    
    <label for="name">Your Name:</label>
    <input type="text" name="yourName" id="name"/>
    <input type="submit" value="Submit"/>
</form>
```

### Rendering fields manually

> templates/contact/nameform.html

```html
<body>
    <form action="{% url 'nameview' %}" method="post">
        {% csrf_token %}
        {{ form.non_field_errors }}
        {{ form.yourName.errors }}
        <label for="{{ form.yourName.id_for_label }}">{{ form.label_tag }}</label>
        {{ form.yourName }}
        <input type="submit" value="Submit"/>
    </form>
</body>

```

More fields [Django Form Field](https://docs.djangoproject.com/en/2.1/topics/forms/#looping-over-the-form-s-fields).

### Adding Style classes for forms

> forms.py

```python

class FormView(forms.Form):
    context_styles = {
        'class': 'input_field',
        'placeholder': 'Enter Your name'
    }
    username = forms.CharField(label = "Name:", required = True, max_length=120, widget=forms.TextInput(attrs = context_styles))

```

### Custom Error checking

it check errors on calling form.is_valid()

> forms.py

```python

class FormView(forms.Form):
    context_styles = {
        'class': 'input_field',
        'placeholder': 'Enter Your name'
    }
    username = forms.CharField(label = "Name:", required = True, max_length=120, widget=forms.TextInput(attrs = context_styles))

    # For checking whole form items non_field_errors
    def clean(self):
        username = self.cleaned_data['username']
        if len(username) < 100:
            raise forms.ValidationError("you can't go below 100", code="field1",)
        return  self.cleaned_data

    # If we want to check each field items
    def clean_username(self):
        username = self.cleaned_data['username']
        if len(username) < 5:
            raise forms.ValidationError("Less than 5", code="username5",)
        return  username


```

### Image Field

> project/settings.py

```python

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

```

> project/urls.py

```python
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

> app/models.py

```python
image = models.ImageField(upload_to='documents/', blank = True)
```
> app/forms.py

```python
image = models.ImageField()
```

> app/views.py

```python

def post(self, request):
    form = FormView(request.POST, request.FILES)
    if form.is_valid():
        form.save()
        # redirect to a new URL:
        return HttpResponseRedirect('/thanks/')
    else :
        print ("Failed validation")

    return render(request, self.template_name, {'form': form1})
```
