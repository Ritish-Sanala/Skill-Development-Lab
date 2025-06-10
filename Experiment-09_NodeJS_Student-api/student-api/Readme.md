# RESTful Student API with Node.js and Express

## 📋 Overview

**Program 9** - A comprehensive RESTful API implementation for managing student data using Node.js and Express.js. This project demonstrates fundamental concepts of backend development, API design, and CRUD operations.

## 🎯 Aim

To implement a fully functional RESTful API for managing student data using Node.js and Express, showcasing modern backend development practices and API design principles.

## 📖 Description

This experiment demonstrates how to build a robust REST API using Node.js and Express framework. The API enables users to perform complete CRUD (Create, Read, Update, Delete) operations on student records through well-defined HTTP endpoints.

### Key Features:
- **In-memory data storage** for simplicity and learning purposes
- **RESTful architecture** following standard HTTP methods and status codes
- **JSON-based communication** for seamless data exchange
- **Express.js routing** for clean and organized endpoint management
- **Error handling** with appropriate HTTP status codes
- **Lightweight implementation** perfect for educational purposes

This backend service simulates a real-world API that could easily be extended with database integration, authentication, and additional features.

## 🏗️ Project Structure

```
student-api/
│
├── server.js          # Main server file with API logic
├── package.json       # Project metadata and dependencies
├── package-lock.json  # Dependency lock file
├── node_modules/      # Installed npm packages
└── README.md          # Project documentation
```

## 🚀 Installation & Setup

### Prerequisites
- **Node.js** (v14.0 or higher)
- **npm** (comes with Node.js)
- **VS Code** or any preferred text editor
- **Postman** or similar API testing tool (optional)

### Step-by-Step Installation

1. **Clone or Create Project Directory**
   ```bash
   mkdir student-api
   cd student-api
   ```

2. **Initialize Node.js Project**
   ```bash
   npm init -y
   ```

3. **Install Dependencies**
   ```bash
   npm install express
   ```

4. **Create Main Server File**
   - Create `server.js` with your API implementation
   - Implement all CRUD operations for student management

5. **Start the Development Server**
   ```bash
   node server.js
   ```

6. **Verify Installation**
   - Open browser and navigate to: `http://localhost:3000/api/students`
   - You should see the API response with student data

## 📡 API Endpoints

### Base URL: `http://localhost:3000/api`

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/students` | Retrieve all students | None |
| GET | `/students/:id` | Get student by ID | None |
| POST | `/students` | Add new student | JSON object |
| PUT | `/students/:id` | Update student by ID | JSON object |
| DELETE | `/students/:id` | Delete student by ID | None |

### Request/Response Examples

#### Get All Students
```http
GET /api/students
```

**Response:**
```json
[
  {
    "id": 1,
    "name": "Swayam",
    "age": 21,
    "course": "Mathematics"
  },
  {
    "id": 2,
    "name": "Ritish",
    "age": 20,
    "course": "Computer Science"
  }
]
```

#### Add New Student
```http
POST /api/students
Content-Type: application/json
```

**Request Body:**
```json
{
  "name": "Swayam",
  "age": 21,
  "course": "Mathematics"
}
```

**Response:**
```json
{
  "id": 1,
  "name": "Swayam",
  "age": 21,
  "course": "Mathematics"
}
```

#### Update Student
```http
PUT /api/students/1
Content-Type: application/json
```

**Request Body:**
```json
{
  "name": "Swayam Kumar",
  "age": 22,
  "course": "Advanced Mathematics"
}
```

#### Delete Student
```http
DELETE /api/students/1
```

**Response:**
```json
{
  "message": "Student deleted successfully"
}
```

## 🐛 Common Issues & Solutions

During development, I encountered several common issues and their solutions:

### Issue 1: Port Already in Use
**Error:** `EADDRINUSE: address already in use :::3000`

**Solution:**
```bash
# Find process using port 3000
lsof -i :3000

# Kill the process (replace PID with actual process ID)
kill -9 <PID>

# Or use a different port in server.js
const PORT = process.env.PORT || 3001;
```

### Issue 2: Cannot POST/PUT Data
**Error:** Request body appears as `undefined`

**Solution:**
Add middleware to parse JSON in `server.js`:
```javascript
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

### Issue 3: CORS Issues (when testing from browser)
**Error:** Cross-Origin Request Blocked

**Solution:**
Install and configure CORS:
```bash
npm install cors
```

```javascript
const cors = require('cors');
app.use(cors());
```

### Issue 4: Module Not Found
**Error:** `Cannot find module 'express'`

**Solution:**
Ensure you're in the correct directory and run:
```bash
npm install express
```

## 🧪 Testing the API

### Using cURL
```bash
# Get all students
curl -X GET http://localhost:3000/api/students

# Add a student
curl -X POST http://localhost:3000/api/students \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","age":20,"course":"Physics"}'

# Update a student
curl -X PUT http://localhost:3000/api/students/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Smith","age":21,"course":"Advanced Physics"}'

# Delete a student
curl -X DELETE http://localhost:3000/api/students/1
```

### Using Postman
1. Import the API endpoints into Postman
2. Set appropriate headers: `Content-Type: application/json`
3. Test each endpoint with sample data

## 🔧 Extending the Project

### Potential Enhancements:
- **Database Integration** (MongoDB, PostgreSQL)
- **Input Validation** (express-validator)
- **Authentication & Authorization** (JWT)
- **Logging** (winston, morgan)
- **Rate Limiting** (express-rate-limit)
- **API Documentation** (Swagger/OpenAPI)
- **Environment Configuration** (.env files)
- **Unit Testing** (Jest, Mocha)

## 📚 Learning Outcomes

This project teaches:
- RESTful API design principles
- Express.js framework fundamentals
- HTTP methods and status codes
- JSON data handling
- Error handling in Node.js
- Basic server-side routing
- API testing methodologies

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 👨‍💻 Author

**Ritish Sanala**
- GitHub: [@Ritish-Sanala](https://github.com/Ritish-Sanala)
- Email: sanalaritish@gmail.com

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Node.js community for excellent documentation
- Express.js team for the robust framework
- Fellow developers who provided inspiration and guidance

---

**Happy Coding! 🚀**