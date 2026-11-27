# EX.No: 03 DEVELOP A DJANGO-BASED CRUD APPLICATION
## Date:27/11/2026
## AIM

To develop a Django-based web application that performs CRUD (Create, Read, Update, and Delete) operations on student records.

## ALGORITHM

1. Create a Django project and an application.
2. Define the `Student` model with the required fields (Name and Email).
3. Run migrations to create the database table.
4. Configure URL routing for Home, Add, Update, and Delete operations.
5. Create the `home()` view to retrieve and display all student records.
6. Create the `add()` view to insert a new student record into the database.
7. Create the `update()` view to modify an existing student record using its ID.
8. Create the `delete()` view to remove a student record using its ID.
9. Design the HTML page with separate forms/buttons for Add, View, Update, and Delete operations.
10. Run the Django development server and perform all CRUD operations through the web browser.

## PROGRAM
```
models.py
from django.db import models
class Student(models.Model):
    name = models.CharField(max_length=100)
    course = models.CharField(max_length=100)

myapp/forms.py
from django import forms
from .models import Student
class StudentForm(forms.ModelForm):
    class Meta:
        model = Student
        fields = '__all__'

views.py
from django.shortcuts import render, redirect, get_object_or_404
from .models import Student
from .forms import StudentForm

# CREATE
def create(request):
    if request.method == 'POST':
        form = StudentForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('read')
    else:
        form = StudentForm()
    return render(request, 'create.html', {'form': form})

# READ
def read(request):
    data = Student.objects.all()
    return render(request, 'read.html', {'data': data})

# UPDATE
def update(request, id):
    s = get_object_or_404(Student, id=id)
    if request.method == 'POST':
        form = StudentForm(request.POST, instance=s)
        if form.is_valid():
            form.save()
            return redirect('read')
    else:
        form = StudentForm(instance=s)
    return render(request, 'update.html', {'form': form})

# DELETE
def delete(request, id):
    s = get_object_or_404(Student, id=id)
    s.delete()
    return redirect('read')

urls.py

from django.urls import path
from . import views
urlpatterns = [
    path('', views.read, name='read'),
    path('create/', views.create, name='create'),
    path('update/<int:id>/', views.update, name='update'),
    path('delete/<int:id>/', views.delete, name='delete'),
]
```
## OUTPUT
<img width="1454" height="675" alt="{BA7916A7-ED86-49F9-A4B6-18715085F421}" src="https://github.com/user-attachments/assets/83f7996d-50df-4f2f-9f9c-45f5d7e51a55" />



## RESULT

A Django-based CRUD web application was successfully developed to perform Create, Read, Update, and Delete operations on student records using SQLite as the backend database.


