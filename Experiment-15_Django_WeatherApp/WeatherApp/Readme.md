# Django Weather App with Chart.js Visualization


## AIM
To develop a weather application frontend using Django templating that displays current weather information along with a line chart showing past 5 days' temperature trends using Chart.js.

## DESCRIPTION
This experiment demonstrates how to build a weather app using Django templates and Chart.js for visualization. The app fetches weather data, including current temperature, description, and icon, and renders it dynamically on a webpage. It also displays historical temperature data for the past 5 days as a line chart. This project is ideal for beginners to learn Django templating, integration of JavaScript libraries like Chart.js, dynamic data visualization on the frontend, and API integration.

## PROJECT STRUCTURE
```
weather-app/
│
├── weather_project/
│   ├── __init__.py
│   ├── settings.py         # Django project settings
│   ├── urls.py             # Main URL configuration
│   ├── wsgi.py             # WSGI configuration
│   └── asgi.py             # ASGI configuration
│
├── weather_app/
│   ├── __init__.py
│   ├── views.py            # Django view to pass weather data to template
│   ├── urls.py             # App URL configuration
│   ├── models.py           # Database models (if needed)
│   ├── admin.py            # Admin configurations
│   └── apps.py             # App configuration
│
├── templates/
│   └── index.html          # Main HTML template with Django template tags and Chart.js
│
├── static/
│   ├── css/
│   │   └── style.css       # Custom CSS styles
│   ├── js/
│   │   └── weather.js      # Custom JavaScript functions
│   └── images/
│       └── weather-icons/  # Weather icon assets
│
├── manage.py               # Django management script
└── README.md               # Project documentation
```

## INSTALLATION & SETUP

### PREREQUISITES
• Python 3.8 or higher  
• Django 4.x or higher (`pip install django`)  
• Requests library (`pip install requests`)  
• OpenWeatherMap API key (free registration required)  
• Basic knowledge of Django templating and Python  
• Chart.js included via CDN (no extra installation needed)  

### STEPS TO RUN THE PROJECT

1. **Create Django Project & App**
   ```bash
   django-admin startproject weather_project
   cd weather_project
   python manage.py startapp weather_app
   ```

2. **Install Required Dependencies**
   ```bash
   pip install django requests
   ```

3. **Setup URL and Views**
   - Add URL route to render the weather template
   - Create view function to fetch and pass weather data (current + historical) as context
   - Configure API integration for weather data

4. **Create Template**
   - Place `index.html` inside `templates/` folder
   - Use Django templating tags to insert weather data into HTML and JavaScript

5. **Configure Settings**
   - Add app to `INSTALLED_APPS` in `settings.py`
   - Configure templates directory
   - Add static files configuration

6. **Run Migrations** (if using models)
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

7. **Run the Server**
   ```bash
   python manage.py runserver
   ```

8. **Visit in Browser**
   Open your browser and go to: `http://127.0.0.1:8000/`

## COMMON ERRORS ENCOUNTERED & SOLUTIONS

### Error 1: ModuleNotFoundError: No module named 'requests'
**Problem:** Requests library not installed for API calls.

**Solution:**
```bash
pip install requests
```

### Error 2: API Key Authentication Failed
**Problem:** Missing or invalid OpenWeatherMap API key.

**Solution:**
- Sign up at https://openweathermap.org/api
- Get your free API key
- Add to Django settings:
```python
# settings.py
WEATHER_API_KEY = 'your_api_key_here'
```

### Error 3: Chart.js not rendering properly
**Problem:** Chart.js CDN not loading or JavaScript errors.

**Solution:**
- Ensure CDN link is correct in template:
```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
```
- Check browser console for JavaScript errors
- Verify data format is correct for Chart.js

### Error 4: Template not found error
**Problem:** Django couldn't locate the index.html template.

**Solution:**
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

### Error 5: CORS Error when fetching weather data
**Problem:** Cross-origin requests blocked by browser.

**Solution:**
- Make API calls from Django backend, not frontend JavaScript
- Pass data through Django context:
```python
def weather_view(request):
    # Fetch data in backend
    weather_data = fetch_weather_api()
    return render(request, 'index.html', {'weather': weather_data})
```

### Error 6: Static files not loading
**Problem:** CSS/JS files not loading properly.

**Solution:**
- Configure static files in `settings.py`:
```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
```
- Load static files in template:
```html
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
```

### Error 7: JSON serialization error in templates
**Problem:** Passing Python data to JavaScript in templates.

**Solution:**
- Use Django's `json_script` filter:
```html
{{ weather_data|json_script:"weather-data" }}
<script>
    const weatherData = JSON.parse(document.getElementById('weather-data').textContent);
</script>
```

### Error 8: Temperature data not displaying in chart
**Problem:** Date formatting or data structure issues.

**Solution:**
- Ensure proper date formatting:
```python
# views.py
formatted_dates = [date.strftime('%Y-%m-%d') for date in date_list]
```
- Verify Chart.js data structure:
```javascript
const chartData = {
    labels: dates,
    datasets: [{
        label: 'Temperature (°C)',
        data: temperatures,
        borderColor: 'rgb(75, 192, 192)',
        tension: 0.1
    }]
};
```

## FEATURES IMPLEMENTED
- ✅ Current weather display with temperature, description, and icon
- ✅ 5-day historical temperature chart using Chart.js
- ✅ Responsive design for mobile and desktop
- ✅ Real-time weather data from OpenWeatherMap API
- ✅ Dynamic data visualization
- ✅ Clean, modern UI design
- ✅ Error handling for API failures

## API INTEGRATION
- **Weather API:** OpenWeatherMap API
- **Endpoints Used:**
  - Current Weather: `https://api.openweathermap.org/data/2.5/weather`
  - Historical Data: `https://api.openweathermap.org/data/2.5/onecall/timemachine`

## TECHNOLOGIES USED
- Python 3.8+
- Django 4.x
- Chart.js (via CDN)
- OpenWeatherMap API
- HTML5 & CSS3
- JavaScript (ES6+)
- Bootstrap (optional for styling)

## CHART.JS CONFIGURATION
```javascript
const config = {
    type: 'line',
    data: chartData,
    options: {
        responsive: true,
        plugins: {
            title: {
                display: true,
                text: 'Temperature Trend - Last 5 Days'
            }
        },
        scales: {
            y: {
                beginAtZero: false,
                title: {
                    display: true,
                    text: 'Temperature (°C)'
                }
            }
        }
    }
};
```

## FUTURE ENHANCEMENTS
- Add weather forecast for upcoming days
- Implement geolocation-based weather detection
- Add more weather parameters (humidity, wind speed, pressure)
- Include weather alerts and notifications
- Add dark/light theme toggle
- Implement caching for API responses
- Add multiple city comparison feature

## ENVIRONMENT VARIABLES
Create a `.env` file for sensitive data:
```
WEATHER_API_KEY=your_openweathermap_api_key
DEBUG=True
SECRET_KEY=your_django_secret_key
```

## LICENSE
This project is licensed under the MIT License.

---
*Developed as part of Django and data visualization learning journey by Ritish Sanala*