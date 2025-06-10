# Django TODO App - Backend Implementation

**Developer:** Ritish Sanala (Ritish-Sanala)  
**Contact:** sanalaritish@gmail.com

## AIM
To develop a TODO list application backend using Django, allowing users to add, view, and manage tasks with a clean and functional web interface.

## DESCRIPTION
This experiment demonstrates how to build a server-rendered TODO application using Django. The app allows users to manage tasks—such as adding new tasks, viewing task lists, marking tasks as complete, and deleting tasks—using Django's powerful Model-View-Template (MVT) architecture. It showcases key Django concepts like models, views, templates, forms, URL routing, and CRUD operations. This project is ideal for beginners to learn about Django's ORM, template rendering, form handling, and basic backend development.

## PROJECT STRUCTURE
```
django_todo_app/
│
├── todo_project/           # Project settings folder
│   ├── __init__.py
│   ├── settings.py         # Project configuration
│   ├── urls.py             # Root URL configuration
│   ├── wsgi.py             # WSGI entry point
│   └── asgi.py             # ASGI entry point
│
├── todo/                   # Main app folder
│   ├── migrations/
│   │   ├── __init__.py
│   │   └── 0001_initial.py # Database migration files
│   ├── templates/
│   │   └── todo/
│   │       ├── index.html  # Main TODO list template
│   │       ├── add_task.html # Add task form template
│   │       └── base.html   # Base template
│   ├── static/
│   │   └── todo/
│   │       ├── css/
│   │       │   └── style.css # Custom styles
│   │       └── js/
│   │           └── main.js  # JavaScript functionality
│   ├── __init__.py
│   ├── admin.py            # Admin interface
│   ├── apps.py             # App configuration
│   ├── models.py           # Task model
│   ├── views.py            # Logic for handling requests
│   ├── urls.py             # URL routing for app
│   ├── forms.py            # Django forms
│   └── tests.py            # Unit tests
│
├── media/                  # Media files (if needed)
├── db.sqlite3              # SQLite database
├── manage.py               # Django management script
├── requirements.txt        # Project dependencies
└── README.md               # Project documentation
```

## INSTALLATION & SETUP

### PREREQUISITES
• Python 3.8 or higher  
• pip (Python package installer)  
• Virtual environment (recommended)  
• VS Code or any code editor  
• Basic knowledge of Django and Python  

### STEPS TO RUN THE PROJECT

1. **Create Virtual Environment (Recommended)**
   ```bash
   python -m venv todo_env
   source todo_env/bin/activate  # Linux/Mac
   # or
   todo_env\Scripts\activate     # Windows
   ```

2. **Create Project and App**
   ```bash
   django-admin startproject todo_project
   cd todo_project
   python manage.py startapp todo
   ```

3. **Install Django**
   ```bash
   pip install django
   pip freeze > requirements.txt
   ```

4. **Configure Settings**
   Add 'todo' to INSTALLED_APPS in `settings.py`:
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'todo',  # Add this line
   ]
   ```

5. **Define Model in todo/models.py**
   ```python
   from django.db import models
   from django.utils import timezone

   class Task(models.Model):
       title = models.CharField(max_length=200)
       description = models.TextField(blank=True)
       completed = models.BooleanField(default=False)
       created_at = models.DateTimeField(default=timezone.now)
       updated_at = models.DateTimeField(auto_now=True)

       class Meta:
           ordering = ['-created_at']

       def __str__(self):
           return self.title
   ```

6. **Create and Apply Migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

7. **Set Up Views, Templates, and URLs**
   - In `views.py`, write logic to render tasks
   - Create `templates/todo/index.html`
   - Map URLs in `todo/urls.py` and include in `todo_project/urls.py`

8. **Create Superuser (Optional)**
   ```bash
   python manage.py createsuperuser
   ```

9. **Run the Project**
   ```bash
   python manage.py runserver
   ```

10. **Visit in Browser**
    Open your browser and go to: `http://127.0.0.1:8000/`

## COMMON ERRORS ENCOUNTERED & SOLUTIONS

### Error 1: App not found in INSTALLED_APPS
**Problem:** Created app wasn't registered in Django settings.

**Solution:**
Add the app to `INSTALLED_APPS` in `settings.py`:
```python
INSTALLED_APPS = [
    # ... other apps
    'todo',
]
```

### Error 2: No such table: todo_task
**Problem:** Forgot to run migrations after creating models.

**Solution:**
```bash
python manage.py makemigrations todo
python manage.py migrate
```

### Error 3: TemplateDoesNotExist at /
**Problem:** Template directory not configured properly.

**Solution:**
- Ensure `templates/todo/` directory exists
- Check `settings.py` TEMPLATES configuration:
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        # ... rest of config
    },
]
```

### Error 4: NoReverseMatch error for URL patterns
**Problem:** URL names not matching between `urls.py` and templates.

**Solution:**
- Ensure URL names are consistent:
```python
# todo/urls.py
path('', views.task_list, name='task_list'),
path('add/', views.add_task, name='add_task'),
```
```html
<!-- In templates -->
<a href="{% url 'add_task' %}">Add Task</a>
```

### Error 5: CSRF verification failed
**Problem:** Missing CSRF token in forms.

**Solution:**
Add CSRF token to all POST forms:
```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Save Task</button>
</form>
```

### Error 6: AttributeError: 'Task' object has no attribute 'get_absolute_url'
**Problem:** Trying to use URL reverse without proper configuration.

**Solution:**
Add `get_absolute_url` method to model or use proper URL reversing:
```python
# In models.py
def get_absolute_url(self):
    from django.urls import reverse
    return reverse('task_detail', kwargs={'pk': self.pk})
```

### Error 7: Static files not loading
**Problem:** CSS and JavaScript files not loading properly.

**Solution:**
- Configure static files in `settings.py`:
```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```
- In templates:
```html
{% load static %}
<link rel="stylesheet" href="{% static 'todo/css/style.css' %}">
```

### Error 8: Form validation errors not displaying
**Problem:** Form errors not being passed to template properly.

**Solution:**
- In views.py:
```python
def add_task(request):
    if request.method == 'POST':
        form = TaskForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('task_list')
    else:
        form = TaskForm()
    return render(request, 'todo/add_task.html', {'form': form})
```

### Error 9: Migration conflicts
**Problem:** Multiple migration files conflicting with each other.

**Solution:**
```bash
# Reset migrations (use with caution)
python manage.py migrate todo zero
rm todo/migrations/00*.py
python manage.py makemigrations todo
python manage.py migrate
```

## FEATURES IMPLEMENTED
- ✅ Add new tasks with title and description
- ✅ View all tasks in a clean list format
- ✅ Mark tasks as complete/incomplete
- ✅ Delete tasks
- ✅ Edit existing tasks
- ✅ Admin interface for task management
- ✅ Responsive web design
- ✅ Form validation and error handling
- ✅ CRUD operations using Django ORM

## DJANGO CONCEPTS DEMONSTRATED
- **Models:** Task model with proper field types and validation
- **Views:** Function-based views for handling HTTP requests
- **Templates:** HTML templates with Django template language
- **URLs:** URL routing and reverse URL lookups
- **Forms:** Django forms for data input and validation
- **Admin:** Django admin interface for backend management
- **ORM:** Database operations using Django's Object-Relational Mapping

## TECHNOLOGIES USED
- Python 3.8+
- Django 4.x
- SQLite (default database)
- HTML5 & CSS3
- JavaScript (optional enhancements)
- Bootstrap (optional for styling)

## SAMPLE VIEWS CODE
```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Task
from .forms import TaskForm

def task_list(request):
    tasks = Task.objects.all()
    return render(request, 'todo/index.html', {'tasks': tasks})

def add_task(request):
    if request.method == 'POST':
        form = TaskForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('task_list')
    else:
        form = TaskForm()
    return render(request, 'todo/add_task.html', {'form': form})
```

## FUTURE ENHANCEMENTS
- Add user authentication and authorization
- Implement task categories and tags
- Add due dates and reminders
- Include task priority levels
- Add search and filter functionality
- Implement task sharing between users
- Add REST API endpoints
- Include task statistics and analytics

## TESTING
```bash
# Run tests
python manage.py test

# Run specific app tests
python manage.py test todo

# Create test data
python manage.py shell
>>> from todo.models import Task
>>> Task.objects.create(title="Test Task", description="Test Description")
```

## DEPLOYMENT CONSIDERATIONS
- Set `DEBUG = False` in production
- Configure proper database (PostgreSQL/MySQL)
- Set up static file serving
- Configure environment variables
- Use proper web server (Gunicorn + Nginx)

## LICENSE
This project is licensed under the MIT License.
