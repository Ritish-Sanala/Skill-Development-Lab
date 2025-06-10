# Secure Student API with JWT Authentication

## 📋 Overview

**Program 10** - A comprehensive secure RESTful API implementation for managing student data with JWT-based authentication using Node.js, Express.js, and MongoDB. This project demonstrates enterprise-level security practices and modern backend development.

## 🎯 Aim

To develop a secure RESTful Student API using Node.js, Express, MongoDB, and JWT (JSON Web Token) for robust user authentication and authorization.

## 📖 Description

This advanced experiment demonstrates how to build a production-ready secure REST API using Node.js and Express with JWT-based authentication system. The API ensures that only authenticated users can perform CRUD (Create, Read, Update, Delete) operations on student records through protected routes.

### Key Features:
- **JWT Authentication** - Secure token-based user authentication
- **MongoDB Integration** - Persistent data storage with Mongoose ODM
- **Protected Routes** - Middleware-based route protection
- **Password Encryption** - BCrypt hashing for secure password storage
- **RESTful Architecture** - Following REST API design principles
- **Environment Configuration** - Secure configuration management
- **Modular Structure** - Clean separation of concerns
- **Error Handling** - Comprehensive error management
- **Security Best Practices** - Production-ready security implementation

This implementation showcases professional backend development practices suitable for real-world applications.

## 🏗️ Project Structure

```
student-api-jwt/
│
├── controllers/
│   ├── authController.js      # Authentication logic & user login
│   └── studentController.js   # CRUD operations for students
│
├── middleware/
│   └── authMiddleware.js      # JWT token verification middleware
│
├── models/
│   ├── User.js               # MongoDB User schema
│   └── Student.js            # MongoDB Student schema
│
├── routes/
│   ├── authRoutes.js         # Authentication routes (/api/auth)
│   └── studentRoutes.js      # Student CRUD routes (/api/students)
│
├── .env                      # Environment variables (secure config)
├── .gitignore               # Git ignore file
├── app.js                   # Main application entry point
├── package.json             # Project metadata and dependencies
├── package-lock.json        # Dependency lock file
└── README.md                # Project documentation
```

## 🚀 Installation & Setup

### Prerequisites
- **Node.js** (v14.0 or higher)
- **MongoDB** (Atlas Cloud or Local Installation)
- **npm** (comes with Node.js)
- **VS Code** or preferred code editor
- **Postman** or similar API testing tool
- **Git** (for version control)

### Step-by-Step Installation

1. **Create Project Directory**
   ```bash
   mkdir student-api-jwt
   cd student-api-jwt
   ```

2. **Initialize Node.js Project**
   ```bash
   npm init -y
   ```

3. **Install Required Dependencies**
   ```bash
   npm install express mongoose jsonwebtoken dotenv bcryptjs cors helmet
   ```

4. **Install Development Dependencies**
   ```bash
   npm install --save-dev nodemon
   ```

5. **Create Project Structure**
   ```bash
   mkdir controllers middleware models routes
   touch app.js .env .gitignore
   ```

6. **Configure Environment Variables**
   Create `.env` file in the root directory:
   ```env
   PORT=5000
   MONGO_URI=mongodb://localhost:27017/student-api
   # For MongoDB Atlas: mongodb+srv://username:password@cluster.mongodb.net/database
   JWT_SECRET=your_super_secret_jwt_key_here_make_it_long_and_complex
   JWT_EXPIRE=7d
   NODE_ENV=development
   ```

7. **Update package.json Scripts**
   ```json
   {
     "scripts": {
       "start": "node app.js",
       "dev": "nodemon app.js",
       "test": "echo \"Error: no test specified\" && exit 1"
     }
   }
   ```

8. **Start Development Server**
   ```bash
   npm run dev
   ```

9. **Verify Installation**
   - Server should start on: `http://localhost:5000`
   - Check MongoDB connection in console logs

## 📡 API Endpoints

### Base URL: `http://localhost:5000/api`

| Method | Endpoint | Description | Auth Required | Request Body |
|--------|----------|-------------|---------------|--------------|
| POST | `/auth/register` | Register new user | No | JSON |
| POST | `/auth/login` | User login | No | JSON |
| GET | `/students` | Get all students | Yes | None |
| GET | `/students/:id` | Get student by ID | Yes | None |
| POST | `/students` | Create new student | Yes | JSON |
| PUT | `/students/:id` | Update student | Yes | JSON |
| DELETE | `/students/:id` | Delete student | Yes | None |

### Authentication Endpoints

#### Register User
```http
POST /api/auth/register
Content-Type: application/json
```

**Request Body:**
```json
{
  "name": "Ritish Sanala",
  "email": "sanalaritish@gmail.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "User registered successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "64f5a1b2c3d4e5f6a7b8c9d0",
    "name": "Ritish Sanala",
    "email": "sanalaritish@gmail.com"
  }
}
```

#### Login User
```http
POST /api/auth/login
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "sanalaritish@gmail.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "64f5a1b2c3d4e5f6a7b8c9d0",
    "name": "Ritish Sanala",
    "email": "sanalaritish@gmail.com"
  }
}
```

### Protected Student Endpoints

#### Get All Students
```http
GET /api/students
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Response:**
```json
{
  "success": true,
  "count": 2,
  "data": [
    {
      "_id": "64f5a1b2c3d4e5f6a7b8c9d1",
      "name": "Swayam",
      "age": 21,
      "course": "Mathematics",
      "email": "swayam@example.com",
      "createdAt": "2024-01-15T10:30:00.000Z"
    },
    {
      "_id": "64f5a1b2c3d4e5f6a7b8c9d2",
      "name": "Priya",
      "age": 20,
      "course": "Computer Science",
      "email": "priya@example.com",
      "createdAt": "2024-01-16T14:20:00.000Z"
    }
  ]
}
```

#### Add New Student
```http
POST /api/students
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
```

**Request Body:**
```json
{
  "name": "Swayam Kumar",
  "age": 21,
  "course": "Mathematics",
  "email": "swayam@example.com",
  "phone": "+91-9876543210"
}
```

#### Update Student
```http
PUT /api/students/64f5a1b2c3d4e5f6a7b8c9d1
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
```

#### Delete Student
```http
DELETE /api/students/64f5a1b2c3d4e5f6a7b8c9d1
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## 🐛 Troubleshooting & Common Issues

During development, I encountered several issues and here are the solutions:

### Issue 1: MongoDB Connection Failed
**Error:** `MongooseServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017`

**Solutions:**
```bash
# For local MongoDB - ensure MongoDB is running
sudo systemctl start mongod  # Linux
brew services start mongodb-community  # macOS

# For MongoDB Atlas - check connection string
# Ensure IP is whitelisted and credentials are correct
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/database?retryWrites=true&w=majority
```

### Issue 2: JWT Token Issues
**Error:** `JsonWebTokenError: invalid token` or `TokenExpiredError`

**Solutions:**
```javascript
// Ensure JWT_SECRET is properly set in .env
JWT_SECRET=your_very_long_and_secure_secret_key_at_least_32_characters

// Check token format in headers
Authorization: Bearer YOUR_TOKEN_HERE

// Verify middleware implementation
const authMiddleware = (req, res, next) => {
  const token = req.header('Authorization')?.replace('Bearer ', '');
  if (!token) {
    return res.status(401).json({ message: 'Access denied. No token provided.' });
  }
  // ... rest of verification logic
};
```

### Issue 3: Password Hashing Problems
**Error:** Password comparison always returns false

**Solution:**
```javascript
// In authController.js - ensure proper bcrypt usage
const bcrypt = require('bcryptjs');

// Hashing password during registration
const salt = await bcrypt.genSalt(10);
const hashedPassword = await bcrypt.hash(password, salt);

// Comparing during login
const isMatch = await bcrypt.compare(password, user.password);
```

### Issue 4: CORS Issues
**Error:** `Access to fetch blocked by CORS policy`

**Solution:**
```javascript
// Install and configure CORS
npm install cors

// In app.js
const cors = require('cors');
app.use(cors({
  origin: ['http://localhost:3000', 'http://localhost:3001'],
  credentials: true
}));
```

### Issue 5: Environment Variables Not Loading
**Error:** `Cannot read property of undefined` for process.env variables

**Solution:**
```javascript
// Ensure dotenv is configured at the top of app.js
require('dotenv').config();

// Check .env file format (no spaces around =)
PORT=5000
MONGO_URI=mongodb://localhost:27017/student-api

// Verify .env is not in .gitignore during development
```

### Issue 6: Mongoose Deprecation Warnings
**Error:** Various mongoose deprecation warnings

**Solution:**
```javascript
// Add these options to mongoose connection
mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
```

## 🧪 Testing with Postman

### Setting up Postman Environment
1. Create a new environment in Postman
2. Add variables:
   - `baseURL`: `http://localhost:5000/api`
   - `token`: (will be set after login)

### Testing Workflow
1. **Register a new user** → Copy the returned token
2. **Set token in environment** → Use for subsequent requests
3. **Test protected routes** → All student endpoints
4. **Test token expiration** → Verify security

### Sample Postman Collection
```json
{
  "info": {
    "name": "Student API JWT",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/"
  },
  "item": [
    {
      "name": "Auth",
      "item": [
        {
          "name": "Register",
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"name\": \"Test User\",\n  \"email\": \"test@example.com\",\n  \"password\": \"password123\"\n}"
            },
            "url": {
              "raw": "{{baseURL}}/auth/register",
              "host": ["{{baseURL}}"],
              "path": ["auth", "register"]
            }
          }
        }
      ]
    }
  ]
}
```

## 🔧 Advanced Features & Extensions

### Implemented Security Features:
- **Password Hashing** with BCrypt
- **JWT Token Authentication**
- **Route Protection Middleware**
- **Environment Variable Security**
- **Input Validation**
- **Error Handling**

### Potential Enhancements:
- **Refresh Tokens** for extended sessions
- **Role-Based Access Control** (Admin, Student, Teacher)
- **Rate Limiting** to prevent abuse
- **Email Verification** for user registration
- **Password Reset** functionality
- **API Documentation** with Swagger
- **Unit Testing** with Jest
- **Docker Containerization**
- **Redis Caching** for performance
- **File Upload** for student photos

## 📚 Learning Outcomes

This project teaches:
- JWT-based authentication implementation
- MongoDB integration with Mongoose
- Middleware development and usage
- Password hashing and security
- Protected route implementation
- Environment configuration management
- RESTful API design with security
- Error handling and validation
- Modern Node.js development practices

## 🔒 Security Best Practices Implemented

1. **Password Security**: BCrypt hashing with salt
2. **Token Security**: JWT with expiration
3. **Environment Security**: Sensitive data in .env
4. **Route Protection**: Middleware authentication
5. **Input Validation**: Data sanitization
6. **Error Handling**: No sensitive data exposure
7. **CORS Configuration**: Controlled cross-origin access

## 🤝 Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## 👨‍💻 Author

**Ritish Sanala**
- GitHub: [@Ritish-Sanala](https://github.com/Ritish-Sanala)
- Email: sanalaritish@gmail.com
- LinkedIn: [Connect with me](https://linkedin.com/in/ritish-sanala)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- JWT.io for excellent JWT documentation
- MongoDB team for comprehensive database solutions
- Express.js community for robust framework
- Node.js community for continuous innovation
- Security researchers for best practices guidance

---

**Secure Coding! 🔐**

> "Security is not a product, but a process." - Bruce Schneier