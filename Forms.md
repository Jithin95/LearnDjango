# Html Forms

> form.html

'''html
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
'''
