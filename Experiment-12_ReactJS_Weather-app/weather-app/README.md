# React Weather Services Application

## 📋 Overview

**Program 12** - A comprehensive real-time weather information web application built with React.js, featuring current weather data, historical trends visualization, and modern UI design. This project demonstrates advanced React concepts including API integration, data visualization, and responsive design.

## 🎯 Aim

To develop a fully functional weather information web application using React that fetches current and historical weather data from OpenWeatherMap API and visualizes temperature trends using Chart.js with a modern canvas-style interface.

## 📖 Description

This advanced experiment demonstrates how to create a sophisticated real-time weather service using React.js with professional-grade features. The application provides comprehensive weather information including current conditions and temperature trends from the past 5 days, presented through interactive charts and a modern user interface.

### Key Features:
- **Real-time Weather Data** - Live current weather conditions
- **Historical Weather Trends** - Past 5 days temperature visualization
- **Interactive Charts** - Chart.js powered data visualization
- **Responsive Design** - Mobile-first responsive interface
- **Modern UI/UX** - Canvas-style card-based layout
- **API Integration** - OpenWeatherMap API integration
- **Geolocation Support** - Automatic location detection
- **Search Functionality** - City-based weather search
- **Error Handling** - Robust error management
- **Loading States** - User-friendly loading indicators
- **Data Caching** - Optimized API call management

This project showcases advanced React patterns, asynchronous data handling, third-party API integration, and modern data visualization techniques.

## 🏗️ Project Structure

```
weather-app/
│
├── public/
│   ├── index.html              # Main HTML template
│   ├── favicon.ico             # Weather app icon
│   ├── manifest.json           # PWA manifest
│   └── weather-icons/          # Custom weather icons
│
├── src/
│   ├── components/
│   │   ├── WeatherChart.js     # Chart.js visualization component
│   │   ├── CurrentWeather.js   # Current weather display
│   │   ├── SearchBar.js        # City search component
│   │   ├── WeatherCard.js      # Weather info card component
│   │   ├── LoadingSpinner.js   # Loading indicator
│   │   └── ErrorMessage.js     # Error display component
│   │
│   ├── services/
│   │   ├── weatherService.js   # OpenWeatherMap API service
│   │   ├── geolocation.js      # Geolocation utilities
│   │   └── dataCache.js        # API response caching
│   │
│   ├── hooks/
│   │   ├── useWeather.js       # Custom weather data hook
│   │   └── useGeolocation.js   # Geolocation hook
│   │
│   ├── utils/
│   │   ├── constants.js        # App constants and config
│   │   ├── helpers.js          # Utility functions
│   │   └── dateUtils.js        # Date formatting utilities
│   │
│   ├── styles/
│   │   ├── App.css             # Main application styles
│   │   ├── components.css      # Component-specific styles
│   │   ├── charts.css          # Chart styling
│   │   └── responsive.css      # Mobile responsive styles
│   │
│   ├── App.js                  # Main application component
│   ├── App.css                 # Global app styling
│   ├── index.js                # React DOM entry point
│   ├── index.css               # Global CSS styles
│   └── App.test.js             # Unit tests
│
├── .env                        # Environment variables (API keys)
├── .env.example                # Environment variables template
├── .gitignore                  # Git ignore rules
├── package.json                # Dependencies and scripts
├── package-lock.json           # Dependency lock file
└── README.md                   # Project documentation
```

## 🚀 Installation & Setup

### Prerequisites
- **Node.js** (v14.0 or higher) - [Download here](https://nodejs.org/)
- **npm** or **yarn** package manager
- **OpenWeatherMap API Key** - [Get free API key](https://openweathermap.org/api)
- **VS Code** or preferred code editor
- **Modern web browser** with JavaScript enabled

### Step-by-Step Installation

1. **Create React Application**
   ```bash
   npx create-react-app weather-app
   cd weather-app
   ```

2. **Install Required Dependencies**
   ```bash
   npm install chart.js react-chartjs-2 axios
   npm install react-icons react-router-dom
   npm install date-fns  # For date formatting
   ```

3. **Install Development Dependencies**
   ```bash
   npm install --save-dev @testing-library/jest-dom
   npm install --save-dev eslint-plugin-react-hooks
   ```

4. **Set Up Environment Variables**
   ```bash
   # Create .env file in root directory
   touch .env
   
   # Add your OpenWeatherMap API key
   echo "REACT_APP_WEATHER_API_KEY=your_api_key_here" >> .env
   echo "REACT_APP_WEATHER_BASE_URL=https://api.openweathermap.org/data/2.5" >> .env
   ```

5. **Create Project Structure**
   ```bash
   mkdir src/components src/services src/hooks src/utils src/styles
   ```

6. **Create Component Files**
   ```bash
   touch src/components/WeatherChart.js
   touch src/components/CurrentWeather.js
   touch src/components/SearchBar.js
   touch src/services/weatherService.js
   touch src/hooks/useWeather.js
   ```

7. **Update package.json Scripts**
   ```json
   {
     "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
       "lint": "eslint src/",
       "format": "prettier --write src/"
     }
   }
   ```

8. **Start Development Server**
   ```bash
   npm start
   ```

9. **Access Application**
   - Open browser: `http://localhost:3000`
   - Allow location access for automatic weather detection

## 🔧 Core Implementation

### Weather Service (API Integration)
```javascript
// src/services/weatherService.js
import axios from 'axios';

const API_KEY = process.env.REACT_APP_WEATHER_API_KEY;
const BASE_URL = process.env.REACT_APP_WEATHER_BASE_URL;

class WeatherService {
  // Get current weather by city
  async getCurrentWeather(city) {
    try {
      const response = await axios.get(
        `${BASE_URL}/weather?q=${city}&appid=${API_KEY}&units=metric`
      );
      return response.data;
    } catch (error) {
      throw new Error(`Failed to fetch weather data: ${error.message}`);
    }
  }

  // Get 5-day forecast
  async getForecast(city) {
    try {
      const response = await axios.get(
        `${BASE_URL}/forecast?q=${city}&appid=${API_KEY}&units=metric`
      );
      return response.data;
    } catch (error) {
      throw new Error(`Failed to fetch forecast data: ${error.message}`);
    }
  }

  // Get weather by coordinates
  async getWeatherByCoords(lat, lon) {
    try {
      const response = await axios.get(
        `${BASE_URL}/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`
      );
      return response.data;
    } catch (error) {
      throw new Error(`Failed to fetch weather data: ${error.message}`);
    }
  }
}

export default new WeatherService();
```

### Weather Chart Component
```javascript
// src/components/WeatherChart.js
import React from 'react';
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} from 'chart.js';
import { Line } from 'react-chartjs-2';

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

const WeatherChart = ({ weatherData }) => {
  const options = {
    responsive: true,
    plugins: {
      legend: {
        position: 'top',
      },
      title: {
        display: true,
        text: '5-Day Temperature Trend',
      },
    },
    scales: {
      y: {
        beginAtZero: false,
        title: {
          display: true,
          text: 'Temperature (°C)',
        },
      },
    },
  };

  const data = {
    labels: weatherData.map(item => 
      new Date(item.dt * 1000).toLocaleDateString()
    ),
    datasets: [
      {
        label: 'Temperature (°C)',
        data: weatherData.map(item => item.main.temp),
        borderColor: 'rgb(75, 192, 192)',
        backgroundColor: 'rgba(75, 192, 192, 0.2)',
        tension: 0.1,
      },
    ],
  };

  return (
    <div className="weather-chart">
      <Line options={options} data={data} />
    </div>
  );
};

export default WeatherChart;
```

### Custom Weather Hook
```javascript
// src/hooks/useWeather.js
import { useState, useEffect } from 'react';
import weatherService from '../services/weatherService';

const useWeather = (city) => {
  const [currentWeather, setCurrentWeather] = useState(null);
  const [forecast, setForecast] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  const fetchWeatherData = async (cityName) => {
    setLoading(true);
    setError(null);
    
    try {
      const [current, forecastData] = await Promise.all([
        weatherService.getCurrentWeather(cityName),
        weatherService.getForecast(cityName)
      ]);
      
      setCurrentWeather(current);
      setForecast(forecastData);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    if (city) {
      fetchWeatherData(city);
    }
  }, [city]);

  return {
    currentWeather,
    forecast,
    loading,
    error,
    refetch: fetchWeatherData
  };
};

export default useWeather;
```

## 🐛 Troubleshooting & Common Issues

During development, I encountered several issues and here are the solutions:

### Issue 1: API Key Not Working
**Error:** `401 Unauthorized` or `Invalid API key`

**Solutions:**
```javascript
// Check .env file format (no quotes around values)
REACT_APP_WEATHER_API_KEY=your_actual_api_key_here

// Verify API key is active on OpenWeatherMap
// Restart development server after adding .env
npm start

// Check if API key is loaded correctly
console.log('API Key:', process.env.REACT_APP_WEATHER_API_KEY);
```

### Issue 2: CORS Policy Errors
**Error:** `Access to fetch blocked by CORS policy`

**Solutions:**
```javascript
// This shouldn't happen with OpenWeatherMap API, but if it does:
// 1. Check API endpoint URL
// 2. Verify API key is valid
// 3. Use axios instead of fetch for better error handling

// Correct API endpoint format
const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}`;
```

### Issue 3: Chart.js Not Rendering
**Error:** Charts not displaying or throwing errors

**Solutions:**
```javascript
// Ensure all required Chart.js components are registered
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} from 'chart.js';

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

// Check data format
const data = {
  labels: ['Day 1', 'Day 2', 'Day 3'],
  datasets: [{
    label: 'Temperature',
    data: [20, 25, 22], // Ensure this is an array of numbers
    borderColor: 'rgb(75, 192, 192)',
  }]
};
```

### Issue 4: Geolocation Not Working
**Error:** Geolocation permission denied or not working

**Solutions:**
```javascript
// Check if geolocation is supported
if (!navigator.geolocation) {
  console.error('Geolocation is not supported by this browser.');
  return;
}

// Handle geolocation errors properly
navigator.geolocation.getCurrentPosition(
  (position) => {
    const { latitude, longitude } = position.coords;
    fetchWeatherByCoords(latitude, longitude);
  },
  (error) => {
    console.error('Geolocation error:', error.message);
    // Fallback to default city
    setDefaultCity('London');
  },
  {
    enableHighAccuracy: true,
    timeout: 5000,
    maximumAge: 0
  }
);
```

### Issue 5: Environment Variables Not Loading
**Error:** `process.env.REACT_APP_WEATHER_API_KEY` is undefined

**Solutions:**
```bash
# Ensure .env file is in root directory (same level as package.json)
# Variable names must start with REACT_APP_
# No spaces around the = sign
# Restart development server after changes

# .env file format:
REACT_APP_WEATHER_API_KEY=your_api_key_here
REACT_APP_WEATHER_BASE_URL=https://api.openweathermap.org/data/2.5

# Verify in code:
console.log('Environment variables:', {
  apiKey: process.env.REACT_APP_WEATHER_API_KEY,
  baseUrl: process.env.REACT_APP_WEATHER_BASE_URL
});
```

### Issue 6: Async/Await Errors
**Error:** `Promise` rejection errors or data not loading

**Solutions:**
```javascript
// Proper error handling with async/await
const fetchWeatherData = async (city) => {
  try {
    setLoading(true);
    setError(null);
    
    const response = await weatherService.getCurrentWeather(city);
    setWeatherData(response);
  } catch (error) {
    console.error('Weather fetch error:', error);
    setError(error.message || 'Failed to fetch weather data');
  } finally {
    setLoading(false);
  }
};

// Handle API response format
const processWeatherData = (data) => {
  if (!data || !data.main) {
    throw new Error('Invalid weather data format');
  }
  return data;
};
```

### Issue 7: CSS Styling Issues
**Error:** Canvas-style UI not displaying correctly

**Solutions:**
```css
/* src/styles/App.css - Canvas-style card design */
.weather-card {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  padding: 30px;
  box-shadow: 0 8px 32px rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.18);
  color: white;
  margin: 20px;
}

.glassmorphism {
  background: rgba(255, 255, 255, 0.25);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.18);
}

/* Responsive design */
@media (max-width: 768px) {
  .weather-card {
    margin: 10px;
    padding: 20px;
  }
}
```

## 🎨 UI/UX Design Features

### Canvas-Style Design Elements
```css
/* Modern gradient backgrounds */
.app-background {
  background: linear-gradient(120deg, #a8edea 0%, #fed6e3 100%);
  min-height: 100vh;
}

/* Glassmorphism effects */
.glass-card {
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
}

/* Smooth animations */
.weather-card {
  transition: all 0.3s ease;
}

.weather-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 40px rgba(31, 38, 135, 0.5);
}
```

## 🧪 Testing the Application

### Manual Testing Checklist
- [ ] API key is correctly configured
- [ ] Current weather displays for searched cities
- [ ] 5-day forecast chart renders correctly
- [ ] Geolocation works (with user permission)
- [ ] Error handling works for invalid cities
- [ ] Loading states display properly
- [ ] Responsive design works on mobile
- [ ] Charts are interactive and readable

### Testing Commands
```bash
# Run React tests
npm test

# Test with coverage
npm test -- --coverage --watchAll=false

# Test specific component
npm test WeatherChart.test.js
```

### API Testing
```javascript
// Test API endpoints manually
const testAPI = async () => {
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY&units=metric`
    );
    const data = await response.json();
    console.log('API Test Result:', data);
  } catch (error) {
    console.error('API Test Failed:', error);
  }
};
```

## 📊 Features Implementation

### Current Weather Display
- Real-time temperature, humidity, wind speed
- Weather condition icons
- "Feels like" temperature
- Sunrise/sunset times
- Air pressure and visibility

### Historical Weather Charts
- 5-day temperature trend line chart
- Humidity levels over time
- Wind speed variations
- Interactive chart tooltips
- Responsive chart scaling

### Search Functionality
- City name search
- Auto-suggestions (future enhancement)
- Search history
- Current location detection
- Invalid city error handling

## 🔧 Advanced Features & Extensions

### Current Implementation:
- **OpenWeatherMap API** integration
- **Chart.js** data visualization
- **Responsive design** with canvas-style UI
- **Geolocation** support
- **Error handling** and loading states

### Potential Enhancements:
- **Weather Alerts** and notifications
- **Hourly Forecast** detailed view
- **Weather Maps** integration
- **Favorites** city list
- **Dark/Light Mode** toggle
- **PWA Features** for offline access
- **Push Notifications** for weather updates
- **Weather Widgets** for different data types
- **Social Sharing** of weather information
- **Historical Data** comparison
- **Weather Animation** effects
- **Voice Search** integration

## 📚 Learning Outcomes

This project teaches:
- **API Integration** with real-world services
- **Asynchronous Programming** with async/await
- **Data Visualization** with Chart.js
- **React Hooks** and custom hook creation
- **State Management** for complex data
- **Error Handling** and user experience
- **Responsive Design** principles
- **Environment Variables** and security
- **Modern CSS** techniques and animations
- **Performance Optimization** for API calls

## 🚀 Deployment Guide

### Environment Setup for Production
```bash
# Build for production
npm run build

# Set production environment variables
REACT_APP_WEATHER_API_KEY=your_production_api_key
```

### Deploy to Netlify
1. Build: `npm run build`
2. Deploy `build` folder to Netlify
3. Set environment variables in Netlify dashboard
4. Configure redirects for SPA

### Deploy to Vercel
```bash
npm install -g vercel
vercel --prod
```

### Deploy to GitHub Pages
```bash
npm install --save-dev gh-pages

# Add to package.json
"homepage": "https://ritish-sanala.github.io/weather-app",
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}

npm run deploy
```

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/weather-maps`
3. **Commit** changes: `git commit -m 'Add weather maps feature'`
4. **Push** to branch: `git push origin feature/weather-maps`
5. **Open** a Pull Request

### Development Guidelines:
- Follow React best practices
- Use TypeScript for type safety (future migration)
- Write unit tests for components
- Follow ESLint configuration
- Update documentation for new features

## 👨‍💻 Author

**Ritish Sanala**
- GitHub: [@Ritish-Sanala](https://github.com/Ritish-Sanala)
- Email: sanalaritish@gmail.com
- LinkedIn: [Connect with me](https://linkedin.com/in/ritish-sanala)
- Portfolio: [Visit my portfolio](https://ritish-sanala.github.io)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **OpenWeatherMap** for providing the weather API
- **Chart.js Team** for the excellent charting library
- **React Team** for the amazing framework
- **Create React App** for the development setup
- **MDN Web Docs** for comprehensive web APIs documentation
- **Stack Overflow Community** for problem-solving support

## 📖 API Reference

### OpenWeatherMap API Endpoints Used:
- **Current Weather**: `/weather?q={city}&appid={API_KEY}`
- **5-Day Forecast**: `/forecast?q={city}&appid={API_KEY}`
- **Weather by Coordinates**: `/weather?lat={lat}&lon={lon}&appid={API_KEY}`

### Rate Limits:
- Free tier: 1,000 calls/day, 60 calls/minute
- Paid plans available for higher usage

---

**Happy Weather Coding! 🌤️**

> "Weather is a great metaphor for life - sometimes it's good, sometimes it's bad, but it always changes." - Terri Guillemets