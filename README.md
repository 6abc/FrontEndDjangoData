# FrontEndDjangoData

# models.py
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

# admin.py
from django.contrib import admin
from .models import Student

# Register your models here.
@admin.register(Student)
class PostAdmin(admin.ModelAdmin):
    list_display = ['name', 'Subscribers', 'Views', 'Videos']
    list_filter = ['name']
    search_fields = ['name', 'Subscribers', 'Views', 'Videos']

# views.py
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

# urls.py
from django.urls import path
from .views import student_list
from . import views 

urlpatterns = [
    path('', views.student_list, name="home"),  # Handle GET requests to the root URL
]
