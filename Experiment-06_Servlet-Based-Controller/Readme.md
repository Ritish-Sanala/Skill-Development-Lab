# Shopping Cart Servlet Application

## Project Overview

**Program 6** - A comprehensive Java Servlet-based controller for a shopping cart application that demonstrates the Model-View-Controller (MVC) architecture pattern.

## Objective

To design and implement a Java Servlet controller that seamlessly connects a front-end shopping cart application with a MySQL database backend, showcasing modern web development practices using the MVC architecture.

## Architecture & Technology Stack

### Backend
- **Java Servlets** - Controller layer handling HTTP requests
- **MySQL Database** - Data persistence layer
- **Apache Tomcat** - Web server and servlet container

### Frontend
- **HTML5** - Structure and presentation
- **Canvas API** - Dynamic UI components
- **CSS3** - Styling and responsive design

### Development Environment
- **IDE**: Visual Studio Code with Java Extension Pack
- **Build Tool**: Manual compilation with javac
- **Database**: MySQL Server

## Project Structure

```
shopping-cart/
│
├── src/
│   └── servlets/
│       └── ProductServlet.java         # Main servlet controller
│
├── WebContent/
│   ├── index.html                      # Landing page with canvas UI
│   ├── catalog.html                    # Product catalog with dynamic loading
│   ├── css/
│   │   └── styles.css                  # Application stylesheets
│   ├── js/
│   │   └── app.js                      # Client-side JavaScript
│   └── WEB-INF/
│       ├── web.xml                     # Servlet configuration
│       ├── classes/                    # Compiled servlet classes
│       └── lib/
│           └── mysql-connector-j-9.3.0.jar
│
├── bin/                                # Compiled .class files
├── lib/
│   └── javax.servlet-api-4.0.1.jar    # Servlet API dependency
└── README.md                           # Project documentation
```

## Prerequisites

Before setting up the project, ensure you have the following installed:

- **Java JDK 8 or higher**
- **Apache Tomcat 9.0+**
- **MySQL Server 8.0+**
- **Visual Studio Code** with Java Extension Pack
- **MySQL Workbench** (optional, for database management)

## Installation & Setup Guide

### Step 1: Database Configuration

1. **Create Database and Table**:
```sql
CREATE DATABASE shopping_cart;
USE shopping_cart;

CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    description TEXT,
    stock_quantity INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample data
INSERT INTO products (name, price, description, stock_quantity) VALUES
('Laptop', 999.99, 'High-performance laptop', 10),
('Smartphone', 699.99, 'Latest smartphone model', 25),
('Headphones', 199.99, 'Wireless noise-canceling headphones', 15),
('Tablet', 399.99, '10-inch tablet with stylus', 8);
```

### Step 2: Project Setup

1. **Download Required JARs**:
   - `mysql-connector-j-9.3.0.jar` (MySQL JDBC Driver)
   - `javax.servlet-api-4.0.1.jar` (Servlet API)

2. **Place JARs in Correct Locations**:
   ```bash
   # For compilation
   cp mysql-connector-j-9.3.0.jar lib/
   cp javax.servlet-api-4.0.1.jar lib/
   
   # For runtime
   cp mysql-connector-j-9.3.0.jar WebContent/WEB-INF/lib/
   ```

### Step 3: Compilation

```bash
# Navigate to project root
cd shopping-cart/

# Compile the servlet
javac -cp "lib/*" -d bin src/servlets/ProductServlet.java

# Verify compilation
ls bin/servlets/
```

### Step 4: Deployment

1. **Copy to Tomcat**:
```bash
# Copy web content
cp -r WebContent/ $CATALINA_HOME/webapps/shopping-cart/

# Copy compiled classes
cp -r bin/* $CATALINA_HOME/webapps/shopping-cart/WEB-INF/classes/
```

2. **Start Tomcat**:
```bash
$CATALINA_HOME/bin/startup.sh    # Linux/Mac
$CATALINA_HOME/bin/startup.bat   # Windows
```

3. **Access Application**:
   - Home Page: `http://localhost:8080/shopping-cart/index.html`
   - Product API: `http://localhost:8080/shopping-cart/products`

## Configuration Files

### web.xml Configuration
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <display-name>Shopping Cart Application</display-name>
    
    <servlet>
        <servlet-name>ProductServlet</servlet-name>
        <servlet-class>servlets.ProductServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>ProductServlet</servlet-name>
        <url-pattern>/products</url-pattern>
    </servlet-mapping>
    
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
</web-app>
```

## Common Issues & Solutions

### Issue 1: ClassNotFoundException for MySQL Driver
**Error**: `java.lang.ClassNotFoundException: com.mysql.cj.jdbc.Driver`

**Solution**:
```bash
# Ensure MySQL connector is in the correct location
cp mysql-connector-j-9.3.0.jar WebContent/WEB-INF/lib/
# Restart Tomcat after copying
```

### Issue 2: Compilation Errors
**Error**: `package javax.servlet does not exist`

**Solution**:
```bash
# Verify servlet-api.jar is in lib/ folder
# Use correct classpath during compilation
javac -cp "lib/*:." -d bin src/servlets/ProductServlet.java
```

### Issue 3: Database Connection Issues
**Error**: `Communications link failure`

**Solution**:
```java
// Update connection string in ProductServlet.java
String url = "jdbc:mysql://localhost:3306/shopping_cart?useSSL=false&serverTimezone=UTC";
String username = "your_mysql_username";
String password = "your_mysql_password";
```

### Issue 4: 404 Error on Servlet Access
**Error**: `HTTP Status 404 – Not Found`

**Solution**:
- Verify servlet mapping in `web.xml`
- Check if compiled classes are in `WEB-INF/classes/`
- Ensure Tomcat is running and application is deployed

### Issue 5: Port Already in Use
**Error**: `Port 8080 already in use`

**Solution**:
```bash
# Find process using port 8080
lsof -i :8080  # Mac/Linux
netstat -ano | findstr :8080  # Windows

# Kill the process or change Tomcat port in server.xml
```

## API Endpoints

| Method | Endpoint | Description | Response |
|--------|----------|-------------|----------|
| GET | `/products` | Retrieve all products | JSON array of products |
| GET | `/products?id={id}` | Get specific product | JSON object |

## Features

- **MVC Architecture**: Clean separation of concerns
- **Database Integration**: MySQL connectivity with JDBC
- **RESTful API**: JSON responses for product data
- **Responsive Design**: Canvas-based UI components
- **Error Handling**: Comprehensive exception management
- **Connection Pooling**: Efficient database connections

## Testing

### Manual Testing
1. Start the application
2. Navigate to `http://localhost:8080/shopping-cart/`
3. Click on "View Products" to test servlet integration
4. Verify product data loads from database

### Database Testing
```sql
-- Test data insertion
INSERT INTO products (name, price, description, stock_quantity) 
VALUES ('Test Product', 99.99, 'Test Description', 5);

-- Verify servlet can retrieve data
SELECT * FROM products;
```

## Future Enhancements

- [ ] Add shopping cart functionality (add/remove items)
- [ ] Implement user authentication and sessions
- [ ] Add order management system
- [ ] Integrate payment gateway
- [ ] Add product search and filtering
- [ ] Implement REST API for mobile apps

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For questions, suggestions, or collaboration opportunities:

- **Email**: sanalaritish@gmail.com
- **GitHub**: [@Ritish-Sanala](https://github.com/Ritish-Sanala)

---

*This project was developed as part of a web development course to demonstrate servlet-based MVC architecture and database integration patterns.*