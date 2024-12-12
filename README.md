# FrontEndDjangoData

# models.py
```sh
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=255)
    Subscribers = models.FloatField()
    Videos = models.FloatField()
    Views = models.FloatField()

    def calculate_average(self):
        return self.Subscribers + self.Videos + self.Views

    def __str__(self):
        return f"{self.name} | {(self.Subscribers+self.Videos+self.Views)}"
```

# admin.py
```sh
from django.contrib import admin
from .models import Student

# Register your models here.
@admin.register(Student)
class PostAdmin(admin.ModelAdmin):
    list_display = ['name', 'Subscribers', 'Views', 'Videos']
    list_filter = ['name']
    search_fields = ['name', 'Subscribers', 'Views', 'Videos']
```

# views.py
```sh
from django.shortcuts import render
from .models import Student

def calculate_ranking():
    students = Student.objects.all()
    sorted_students = sorted(students, key=lambda student: student.calculate_average(), reverse=True)

    ranking_data = []
    for rank, student in enumerate(sorted_students, start=1):
        ranking_data.append({
            'rank': rank,
            'name': student.name,
            'Subscribers': student.Subscribers,
            'Videos': student.Views,
            'Views': student.Videos,            
            'average_score': student.calculate_average(),         
        })

    return ranking_data

def student_list(request):
    ranking_data = calculate_ranking()
    return render(request, 'student_list.html', {'ranking_data': ranking_data})
```

# urls.py
```sh
from django.urls import path
from .views import student_list
from . import views 

urlpatterns = [
    path('', views.student_list, name="home"),  # Handle GET requests to the root URL
]
```

# Command and Description

| Django Template Tags | Description |
| :--- | :--- |
| `{% load_static %}` | It loads static files |
| `{% static 'assets/js/app.js' %}` | It loads static assets files |
| `{% extends base.html %}` |  It add base.html files |
| `{% include navbar.html %}` | It includes navbar.html files |
| `{% url '<NameFromUrls.py' %}` | It add url location |
| `{% block title %}{% endblock %}` | Allow title for each page |
| `{% block content %}{% endblock %}` | Allow adding content in base.html |
| `{% if user.is_authenticated %} <a href="{% url 'logout' %}">Logout</a> {% else %} <a href="{% url 'login' %}">Login</a> {% endif %}` | If Condition for User Specific Data |
| `{% if x in view_dict %} {{x.<model_var>}} {% empty %} NoData! {% endif %}` | If Condition and accesing django model data |

```
{% extends "base.html" %}
{% load static %}
{% block title %}K8LO | Home{% endblock %}
{% block content %}
  <h1>Content</h1>
{% endblock content %} 
```
