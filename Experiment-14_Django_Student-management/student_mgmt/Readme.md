# Django Student Management System

## AIM
To develop a student management system backend using Django, allowing users to register, login, and access information through a clean and basic Canvas-style UI.

## DESCRIPTION
This experiment demonstrates how to build a basic web application using Django. The project implements user authentication (register and login), and basic navigation through pages like Home, About, and Contact. It showcases Django concepts like URL routing, views, templates, and static file management. This project is ideal for beginners to learn the fundamentals of Django web development including template rendering, view functions, and project structure.

## PROJECT STRUCTURE
```
student_mgmt/
├── core/
│   ├── views.py            # Contains views for all pages
│   ├── urls.py             # App URL routing
│   ├── models.py           # Database models
│   ├── forms.py            # Django forms
│   └── admin.py            # Admin configurations
├── templates/
│   ├── base.html           # Base template with common UI elements
│   ├── home.html           # Home page template
│   ├── about.html          # About page template
│   ├── contact.html        # Contact page template
│   ├── login.html          # Login page template
│   └── register.html       # Registration page template
├── static/
│   ├── css/
│   │   └── style.css       # Custom CSS styles
│   └── js/
│       └── main.js         # JavaScript files
├── student_mgmt/
│   ├── settings.py         # Project settings including installed apps and templates
│   ├── urls.py             # Project-level URL routing
│   ├── wsgi.py             # WSGI configuration
│   └── asgi.py             # ASGI configuration
├── db.sqlite3              # SQLite database file
├── manage.py               # Django project management script
└── README.md               # Project documentation
```

## INSTALLATION & SETUP

### PREREQUISITES
• Python 3.8 or higher  
• Django 5.2  
• VS Code or any preferred code editor  
• Basic knowledge of Python and Django framework  

### STEPS TO RUN THE PROJECT

1. **Clone the repository (or create the project and app as per instructions)**
   ```bash
   git clone <repository-url>
   cd student_mgmt
   ```

2. **Create a virtual environment and activate it (recommended)**
   ```bash
   python -m venv venv
   source venv/bin/activate   # Linux/Mac
   venv\Scripts\activate      # Windows
   ```

3. **Install Django**
   ```bash
   pip install django==5.2
   ```

4. **Run migrations**
   ```bash
   python manage.py migrate
   ```

5. **Create a superuser (optional, for admin access)**
   ```bash
   python manage.py createsuperuser
   ```

6. **Run the development server**
   ```bash
   python manage.py runserver
   ```

7. **Open the app in your browser**
   Visit: `http://127.0.0.1:8000/`

## COMMON ERRORS ENCOUNTERED & SOLUTIONS

### Error 1: ModuleNotFoundError: No module named 'django'
**Problem:** Django wasn't installed or virtual environment wasn't activated.

**Solution:**
```bash
# Activate virtual environment first
source venv/bin/activate  # Linux/Mac
# or
venv\Scripts\activate     # Windows

# Then install Django
pip install django==5.2
```

### Error 2: TemplateDoesNotExist error
**Problem:** Django couldn't find the HTML templates.

**Solution:**
- Ensure `templates` folder exists in the project root
- Check `settings.py` TEMPLATES configuration:
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],  # Add this line
        'APP_DIRS': True,
        # ... rest of config
    },
]
```

### Error 3: NoReverseMatch at /login/
**Problem:** URL name not found in URL patterns.

**Solution:**
- Ensure URL names match in `urls.py`:
```python
# In core/urls.py
path('login/', views.login_view, name='login'),
path('register/', views.register_view, name='register'),
```
- Use correct URL names in templates:
```html
<a href="{% url 'login' %}">Login</a>
```

### Error 4: Static files not loading (CSS/JS)
**Problem:** Static files weren't properly configured.

**Solution:**
- Add to `settings.py`:
```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```
- In templates, load static files:
```html
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
```

### Error 5: CSRF verification failed
**Problem:** Missing CSRF token in forms.

**Solution:**
- Add CSRF token to all forms:
```html
<form method="post">
    {% csrf_token %}
    <!-- form fields -->
</form>
```

### Error 6: Database is locked error
**Problem:** SQLite database was locked by another process.

**Solution:**
```bash
# Stop the server first (Ctrl+C)
# Then restart
python manage.py runserver
```

### Error 7: App not included in INSTALLED_APPS
**Problem:** Created app wasn't registered in settings.

**Solution:**
- Add app to `settings.py`:
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'core',  # Add your app here
]
```

## FEATURES IMPLEMENTED
- ✅ User registration and authentication
- ✅ Login/Logout functionality
- ✅ Home, About, and Contact pages
- ✅ Canvas-style UI design
- ✅ Template inheritance with base.html
- ✅ URL routing and navigation
- ✅ Static file management

## TECHNOLOGIES USED
- Python 3.8+
- Django 5.2
- SQLite (default database)
- HTML5 & CSS3
- Bootstrap (optional for styling)

## FUTURE ENHANCEMENTS
- Add student CRUD operations
- Implement student profile management
- Add course management features
- Implement role-based access control
- Add email verification for registration
- Include grade management system

## PROJECT SETUP COMMANDS
```bash
# Create Django project
django-admin startproject student_mgmt

# Create Django app
python manage.py startapp core

# Make migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Collect static files (for production)
python manage.py collectstatic
```

## LICENSE
This project is licensed under the MIT License.

---
*Developed as part of Django learning journey by Ritish Sanala*