# Todo-List
This is a simple Todo-List project I created to test my skills, and to practice clean code.

READ THIS FOR EXPLANATION

1. Save the code to a file named todo.py.

2.Open a terminal or command prompt, navigate to the directory where you saved the file, and run the following command: python todo.py

3.The code will create a SQLite database named todo.db and a table named 'tasks' within it, if they do not already exist. It will then add two tasks to the tasks table, mark one of them as complete, and delete the other one. The resulting list of tasks will be printed to the console.

To use the app to manage your own todo list, you can modify the code to suit your needs. For example, you might want to add a command-line interface that allows you to add, mark as complete, or delete tasks from the list. You could also create a graphical user interface using a library like PyQt or Tkinter, or create a web-based interface using a framework like Flask or Django.

Here is an example of how you might modify the todo list app code to create a web-based interface using Django:

1. Install Django by running the following command in a terminal or command prompt: 

**
pip install django
**

2. Create a new Django project and app by running the following commands: 

**
django-admin startproject todo_project
cd todo_project
python manage.py startapp todo_app
**


3. In the todo_app directory, create a file named models.py with the following contents: 

**
from django.db import models

class Task(models.Model):
    task = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)
**

4. This defines the Task model, which will be used to represent tasks in the database. In the todo_app directory, create a file named forms.py with the following contents:

**
from django import forms
from .models import Task

class TaskForm(forms.ModelForm):
    class Meta:
        model = Task
        fields = ['task']
**

5. This defines the TaskForm form, which will be used to create and update tasks. In the todo_app directory, create a file named views.py with the following contents:

**
from django.shortcuts import render, redirect
from .models import Task
from .forms import TaskForm

def list_tasks(request):
    tasks = Task.objects.all()
    form = TaskForm()
    context = {'tasks': tasks, 'form': form}
    return render(request, 'todo_app/list_tasks.html', context)

def add_task(request):
    form = TaskForm(request.POST)
    if form.is_valid():
        form.save()
    return redirect('list_tasks')

def complete_task(request, task_id):
    task = Task.objects.get(id=task_id)
    task.completed = True
    task.save()
    return redirect('list_tasks')

def delete_task(request, task_id):
    Task.objects.get(id=task_id).delete()
    return redirect('list_tasks')
**

6. These functions handle the following actions:

- list_tasks(request) displays a list of all tasks and a form for creating a new task.
- add_task(request) adds a new task to the database.
- complete_task(request, task_id) marks a task as complete in the database.
- delete_task(request, task_id) deletes a task from the database.

To use these functions, you will need to create a template for the list_tasks view. In the todo_app directory, create a directory named templates, and within that directory create a file named todo_app/list_tasks.html with the following contents:

**
<h1>TODO List</h1>

<form action="{% url 'add_task' %}" method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <button type="submit">Add Task</button>
</form>

<ul>
  {% for task in tasks %}
    <li>
      {% if task.completed %}
        <strike>{{ task.task }}</strike>
      {% else %}
        {{ task.task }}
      {% endif %}
      <a href="{% url 'complete_task' task.id %}">Complete</a>
      <a href="{% url 'delete_task' task.id %}">Delete</a>
    </li>
  {% endfor %}
</ul>
**

7. This template displays a form for creating a new task and a list of all tasks, along with links for marking a task as complete or deleting it.

Finally, you will need to create URL patterns for the views. In the todo_app directory, create a file named urls.py with the following contents:

**
from django.urls import path
from . import views

urlpatterns = [
    path('', views.list_tasks, name='list_tasks'),
    path('add_task/', views.add_task, name='add_task'),
    path('complete_task/<int:task_id>/', views.complete_task, name='complete_task'),
    path('delete_task/<int:task_id>/', views.delete_task, name='delete_task'),
]
**

8. In the todo_project directory, create a file named urls.py with the following contents:

**
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    path('todo/', include('todo_app.urls')),
]
**

9.This file defines the root URL patterns for the project. It includes the URL patterns for the todo_app app, which are defined in the todo_app/urls.py file.

To run the app, navigate to the todo_project directory in a terminal or command prompt and run the following command:

**
python manage.py runserver
**

NOTE: 

This will start the development server at http://127.0.0.1:8000/. You can visit this URL in your web browser to view and interact with the todo list app.

You can use the Django admin site to manage the tasks in the database. To access the admin site, visit http://127.0.0.1:8000/admin/ and log in using the superuser account you created when you ran the python manage.py createsuperuser command. You can then create, update, and delete tasks using the admin interface.

I hope this helps!
