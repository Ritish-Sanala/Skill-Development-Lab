# Program 5: Java Standalone Application with MySQL (CRUD Operations)

## 📋 Table of Contents
- [Objective](#objective)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Database Configuration](#database-configuration)
- [Project Structure](#project-structure)
- [Implementation Details](#implementation-details)
- [Running the Application](#running-the-application)
- [Common Errors & Solutions](#common-errors--solutions)
- [CRUD Operations Guide](#crud-operations-guide)
- [Best Practices](#best-practices)
- [Testing Guidelines](#testing-guidelines)
- [Future Enhancements](#future-enhancements)
- [Contributing](#contributing)
- [License](#license)
- [Author](#author)

## 🎯 Objective

This project is a comprehensive Java Standalone Application that demonstrates enterprise-level database connectivity using JDBC (Java Database Connectivity). The application performs full CRUD (Create, Read, Update, Delete) operations on a MySQL database, specifically managing a product inventory system with robust error handling and user-friendly interface.

### Learning Outcomes:
- Master JDBC connectivity and database operations
- Implement proper SQL query execution and result handling
- Understand connection pooling and resource management
- Practice exception handling in database operations
- Learn database transaction management

## ✨ Features

### Core Functionality
- **Full CRUD Operations**: Complete Create, Read, Update, Delete functionality
- **MySQL Integration**: Robust database connectivity using JDBC
- **Interactive Console Menu**: User-friendly command-line interface
- **Data Validation**: Input validation and error checking
- **Transaction Management**: Proper database transaction handling
- **Connection Management**: Efficient database connection handling

### Advanced Features
- **Prepared Statements**: SQL injection prevention
- **Batch Operations**: Bulk data processing capabilities
- **Connection Pooling**: Optimized database connections
- **Logging System**: Comprehensive error and operation logging
- **Configuration Management**: External configuration file support
- **Data Export/Import**: CSV and JSON data handling

## 📋 Prerequisites

### Software Requirements
- **MySQL Server 8.0+**: Database management system
- **Java Development Kit (JDK) 11+**: Java runtime and development environment
- **MySQL Connector/J 8.0+**: JDBC driver for MySQL connectivity
- **IDE**: VS Code, IntelliJ IDEA, or Eclipse
- **MySQL Workbench** (Optional): GUI-based database management

### Knowledge Requirements
- Basic understanding of Java programming
- Familiarity with SQL queries and database concepts
- Command-line interface usage
- Basic understanding of JDBC concepts

## 🚀 Installation & Setup

### Step 1: Install Required Software

#### MySQL Server Installation
```bash
# For Ubuntu/Debian
sudo apt update
sudo apt install mysql-server

# For macOS (using Homebrew)
brew install mysql

# For Windows: Download from https://dev.mysql.com/downloads/mysql/
```

#### Java Development Kit
```bash
# Verify Java installation
java -version
javac -version

# If not installed, download from:
# https://www.oracle.com/java/technologies/javase-downloads.html
```

#### MySQL Connector/J
1. Download from: https://dev.mysql.com/downloads/connector/j/
2. Extract the JAR file to your project's `lib` folder
3. Note the exact version for classpath configuration

### Step 2: MySQL Server Configuration

#### Start MySQL Server
```bash
# Linux/macOS
sudo systemctl start mysql
# or
sudo service mysql start

# Windows
net start mysql80
```

#### Secure MySQL Installation
```bash
sudo mysql_secure_installation
```

#### Create Database User (Optional but recommended)
```sql
-- Connect to MySQL as root
mysql -u root -p

-- Create dedicated user for the application
CREATE USER 'crud_user'@'localhost' IDENTIFIED BY 'secure_password123';
GRANT ALL PRIVILEGES ON shoppingdb.* TO 'crud_user'@'localhost';
FLUSH PRIVILEGES;
```

## 🗄️ Database Configuration

### Database Schema Setup
```sql
-- Create the database
CREATE DATABASE IF NOT EXISTS shoppingdb
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

-- Use the database
USE shoppingdb;

-- Create products table with enhanced structure
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL CHECK (price >= 0),
    quantity INT NOT NULL DEFAULT 0 CHECK (quantity >= 0),
    category VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_name (name),
    INDEX idx_category (category)
);

-- Insert sample data
INSERT INTO products (name, description, price, quantity, category) VALUES
('Laptop', 'High-performance laptop for gaming and work', 999.99, 50, 'Electronics'),
('Coffee Mug', 'Ceramic coffee mug with company logo', 12.50, 200, 'Office Supplies'),
('Wireless Mouse', 'Ergonomic wireless mouse with USB receiver', 29.99, 75, 'Electronics'),
('Notebook', 'A4 lined notebook, 200 pages', 5.99, 150, 'Stationery');
```

### Database Connection Configuration
Create `config.properties` file:
```properties
# Database Configuration
db.host=localhost
db.port=3306
db.name=shoppingdb
db.username=crud_user
db.password=secure_password123
db.driver=com.mysql.cj.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/shoppingdb?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true

# Connection Pool Settings
db.pool.initial=5
db.pool.max=20
db.pool.timeout=30000
```

## 📁 Project Structure

```
java-crud-app/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── Main.java                    # Application entry point
│   │   │   ├── model/
│   │   │   │   └── Product.java             # Product entity class
│   │   │   ├── dao/
│   │   │   │   ├── ProductDAO.java          # Data Access Object
│   │   │   │   └── DatabaseConnection.java  # Connection manager
│   │   │   ├── service/
│   │   │   │   └── ProductService.java      # Business logic layer
│   │   │   └── util/
│   │   │       ├── ConfigManager.java       # Configuration handler
│   │   │       └── Logger.java              # Logging utility
│   │   └── resources/
│   │       ├── config.properties            # Configuration file
│   │       └── schema.sql                   # Database schema
├── lib/
│   └── mysql-connector-j-8.0.33.jar        # JDBC driver
├── bin/                                     # Compiled classes
├── logs/                                    # Application logs
├── docs/                                    # Documentation
├── test/                                    # Unit tests
├── README.md
├── .gitignore
└── build.bat / build.sh                     # Build scripts
```

## 💻 Implementation Details

### Product Entity Class
```java
// src/main/java/model/Product.java
public class Product {
    private int id;
    private String name;
    private String description;
    private double price;
    private int quantity;
    private String category;
    private Date createdAt;
    private Date updatedAt;
    
    // Constructors, getters, setters, toString, equals, hashCode
    // Implementation details...
}
```

### Database Connection Manager
```java
// src/main/java/dao/DatabaseConnection.java
import java.sql.*;
import java.util.Properties;

public class DatabaseConnection {
    private static final String CONFIG_FILE = "config.properties";
    private static Properties props;
    
    static {
        loadConfiguration();
    }
    
    public static Connection getConnection() throws SQLException {
        try {
            Class.forName(props.getProperty("db.driver"));
            return DriverManager.getConnection(
                props.getProperty("db.url"),
                props.getProperty("db.username"),
                props.getProperty("db.password")
            );
        } catch (ClassNotFoundException e) {
            throw new SQLException("JDBC Driver not found", e);
        }
    }
    
    private static void loadConfiguration() {
        // Configuration loading implementation
    }
}
```

## 🔧 Running the Application

### Method 1: Command Line (Manual Compilation)
```bash
# Navigate to project directory
cd java-crud-app

# Compile the application
javac -cp ".:lib/mysql-connector-j-8.0.33.jar" -d bin src/main/java/**/*.java

# Run the application (Linux/macOS)
java -cp ".:bin:lib/mysql-connector-j-8.0.33.jar" Main

# Run the application (Windows)
java -cp ".;bin;lib/mysql-connector-j-8.0.33.jar" Main
```

### Method 2: Using Build Scripts

#### Linux/macOS (build.sh)
```bash
#!/bin/bash
echo "Building Java CRUD Application..."

# Create directories
mkdir -p bin logs

# Compile
javac -cp ".:lib/mysql-connector-j-8.0.33.jar" -d bin src/main/java/**/*.java

if [ $? -eq 0 ]; then
    echo "Compilation successful!"
    echo "Running application..."
    java -cp ".:bin:lib/mysql-connector-j-8.0.33.jar" Main
else
    echo "Compilation failed!"
    exit 1
fi
```

#### Windows (build.bat)
```batch
@echo off
echo Building Java CRUD Application...

REM Create directories
if not exist bin mkdir bin
if not exist logs mkdir logs

REM Compile
javac -cp ".;lib/mysql-connector-j-8.0.33.jar" -d bin src/main/java/**/*.java

if %errorlevel% equ 0 (
    echo Compilation successful!
    echo Running application...
    java -cp ".;bin;lib/mysql-connector-j-8.0.33.jar" Main
) else (
    echo Compilation failed!
    pause
    exit /b 1
)
```

### Method 3: IDE Configuration

#### VS Code
1. Install Java Extension Pack
2. Configure `launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "Launch Main",
            "request": "launch",
            "mainClass": "Main",
            "classPaths": ["${workspaceFolder}/bin", "${workspaceFolder}/lib/*"]
        }
    ]
}
```

#### IntelliJ IDEA
1. File → Project Structure → Libraries
2. Add JAR: `lib/mysql-connector-j-8.0.33.jar`
3. Run configuration: Main class = `Main`

## 🐛 Common Errors & Solutions

### 1. ClassNotFoundException: MySQL Driver

**Error**: `java.lang.ClassNotFoundException: com.mysql.cj.jdbc.Driver`

```java
// Problem: JDBC driver not in classpath or wrong driver class
Class.forName("com.mysql.jdbc.Driver"); // Old driver class

// Solution 1: Use correct driver class for MySQL 8.0+
Class.forName("com.mysql.cj.jdbc.Driver");

// Solution 2: Verify JAR file in classpath
// Ensure mysql-connector-j-x.x.x.jar is in lib folder and included in classpath

// Solution 3: Maven dependency (if using Maven)
// <dependency>
//     <groupId>mysql</groupId>
//     <artifactId>mysql-connector-java</artifactId>
//     <version>8.0.33</version>
// </dependency>
```

### 2. SQLException: Access Denied

**Error**: `java.sql.SQLException: Access denied for user 'root'@'localhost'`

```java
// Problem: Incorrect credentials or user permissions
String url = "jdbc:mysql://localhost:3306/shoppingdb";
String user = "root";
String password = "wrong_password";

// Solution 1: Verify MySQL credentials
// Test connection using MySQL command line:
// mysql -u root -p

// Solution 2: Create dedicated database user
/*
CREATE USER 'crud_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON shoppingdb.* TO 'crud_user'@'localhost';
FLUSH PRIVILEGES;
*/

// Solution 3: Update connection parameters
String url = "jdbc:mysql://localhost:3306/shoppingdb?useSSL=false&serverTimezone=UTC";
String user = "crud_user";
String password = "secure_password";
```

### 3. SQLException: Unknown Database

**Error**: `java.sql.SQLSyntaxErrorException: Unknown database 'shoppingdb'`

```sql
-- Problem: Database doesn't exist
-- Solution: Create the database first
CREATE DATABASE IF NOT EXISTS shoppingdb;
USE shoppingdb;

-- Verify database exists
SHOW DATABASES;
```

### 4. Connection Timeout Issues

**Error**: `java.sql.SQLNonTransientConnectionException: Could not create connection to database server`

```java
// Problem: MySQL server not running or connection timeout
// Solution 1: Start MySQL server
// Linux/macOS: sudo service mysql start
// Windows: net start mysql80

// Solution 2: Increase connection timeout in URL
String url = "jdbc:mysql://localhost:3306/shoppingdb?" +
            "useSSL=false&serverTimezone=UTC&" +
            "connectTimeout=60000&socketTimeout=60000";

// Solution 3: Check MySQL server status
// mysql -u root -p -e "SELECT 1"
```

### 5. SQLException: Table Doesn't Exist

**Error**: `java.sql.SQLSyntaxErrorException: Table 'shoppingdb.products' doesn't exist`

```sql
-- Problem: Table not created or wrong table name
-- Solution: Create the table with proper structure
CREATE TABLE IF NOT EXISTS products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    quantity INT NOT NULL DEFAULT 0,
    category VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Verify table exists
SHOW TABLES;
DESCRIBE products;
```

### 6. Resource Leak: Connections Not Closed

**Error**: `java.sql.SQLException: Too many connections` or memory leaks

```java
// Problem: Not closing database resources properly
public void badDatabaseOperation() {
    Connection conn = DriverManager.getConnection(url, user, password);
    Statement stmt = conn.createStatement();
    ResultSet rs = stmt.executeQuery("SELECT * FROM products");
    // Resources not closed - MEMORY LEAK!
}

// Solution: Use try-with-resources (Java 7+)
public void goodDatabaseOperation() {
    String sql = "SELECT * FROM products";
    
    try (Connection conn = DriverManager.getConnection(url, user, password);
         PreparedStatement stmt = conn.prepareStatement(sql);
         ResultSet rs = stmt.executeQuery()) {
        
        while (rs.next()) {
            // Process results
        }
    } catch (SQLException e) {
        logger.error("Database operation failed", e);
        throw new RuntimeException("Failed to fetch products", e);
    }
}
```

### 7. SQL Injection Vulnerabilities

**Error**: Security vulnerability in SQL queries

```java
// Problem: Using string concatenation for SQL queries
public Product getProductById(int id) {
    String sql = "SELECT * FROM products WHERE id = " + id; // VULNERABLE!
    // This allows SQL injection attacks
}

// Solution: Use PreparedStatement with parameters
public Product getProductById(int id) {
    String sql = "SELECT * FROM products WHERE id = ?";
    
    try (Connection conn = getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        
        stmt.setInt(1, id);
        
        try (ResultSet rs = stmt.executeQuery()) {
            if (rs.next()) {
                return mapResultSetToProduct(rs);
            }
        }
    } catch (SQLException e) {
        logger.error("Failed to get product by ID: " + id, e);
        throw new RuntimeException("Database error", e);
    }
    
    return null;
}
```

### 8. Character Encoding Issues

**Error**: Special characters not displaying correctly

```java
// Problem: Character encoding mismatch
String url = "jdbc:mysql://localhost:3306/shoppingdb";

// Solution: Specify character encoding in connection URL
String url = "jdbc:mysql://localhost:3306/shoppingdb?" +
            "useSSL=false&serverTimezone=UTC&" +
            "characterEncoding=UTF-8&useUnicode=true";

// Database should also use UTF-8
/*
CREATE DATABASE shoppingdb
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
*/
```

### 9. Date/Time Handling Issues

**Error**: `java.sql.SQLException: The server time zone value 'UTC' is unrecognized`

```java
// Problem: Time zone not specified in connection URL
String url = "jdbc:mysql://localhost:3306/shoppingdb";

// Solution: Add serverTimezone parameter
String url = "jdbc:mysql://localhost:3306/shoppingdb?" +
            "useSSL=false&serverTimezone=UTC";

// Alternative: Use local system timezone
String url = "jdbc:mysql://localhost:3306/shoppingdb?" +
            "useSSL=false&serverTimezone=" + 
            java.time.ZoneId.systemDefault().getId();
```

### 10. Compilation Classpath Issues

**Error**: `javac: invalid flag: lib/mysql-connector-j-8.0.33.jar`

```bash
# Problem: Incorrect classpath syntax
javac -cp lib/mysql-connector-j-8.0.33.jar src/Main.java

# Solution: Use proper classpath separator
# Linux/macOS (colon separator)
javac -cp ".:lib/mysql-connector-j-8.0.33.jar" -d bin src/**/*.java

# Windows (semicolon separator)
javac -cp ".;lib/mysql-connector-j-8.0.33.jar" -d bin src/**/*.java
```

## 📖 CRUD Operations Guide

### Create Operation
```java
public boolean addProduct(Product product) {
    String sql = "INSERT INTO products (name, description, price, quantity, category) VALUES (?, ?, ?, ?, ?)";
    
    try (Connection conn = getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {
        
        stmt.setString(1, product.getName());
        stmt.setString(2, product.getDescription());
        stmt.setDouble(3, product.getPrice());
        stmt.setInt(4, product.getQuantity());
        stmt.setString(5, product.getCategory());
        
        int rowsAffected = stmt.executeUpdate();
        
        if (rowsAffected > 0) {
            try (ResultSet generatedKeys = stmt.getGeneratedKeys()) {
                if (generatedKeys.next()) {
                    product.setId(generatedKeys.getInt(1));
                }
            }
            return true;
        }
    } catch (SQLException e) {
        logger.error("Failed to add product", e);
    }
    
    return false;
}
```

### Read Operations
```java
public List<Product> getAllProducts() {
    List<Product> products = new ArrayList<>();
    String sql = "SELECT * FROM products ORDER BY name ASC";
    
    try (Connection conn = getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql);
         ResultSet rs = stmt.executeQuery()) {
        
        while (rs.next()) {
            products.add(mapResultSetToProduct(rs));
        }
    } catch (SQLException e) {
        logger.error("Failed to get all products", e);
    }
    
    return products;
}

public Product getProductById(int id) {
    String sql = "SELECT * FROM products WHERE id = ?";
    
    try (Connection conn = getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        
        stmt.setInt(1, id);
        
        try (ResultSet rs = stmt.executeQuery()) {
            if (rs.next()) {
                return mapResultSetToProduct(rs);
            }
        }
    } catch (SQLException e) {
        logger.error("Failed to get product by ID: " + id, e);
    }
    
    return null;
}
```

### Update Operation
```java
public boolean updateProduct(Product product) {
    String sql = "UPDATE products SET name = ?, description = ?, price = ?, quantity = ?, category = ? WHERE id = ?";
    
    try (Connection conn = getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        
        stmt.setString(1, product.getName());
        stmt.setString(2, product.getDescription());
        stmt.setDouble(3, product.getPrice());
        stmt.setInt(4, product.getQuantity());
        stmt.setString(5, product.getCategory());
        stmt.setInt(6, product.getId());
        
        int rowsAffected = stmt.executeUpdate();
        return rowsAffected > 0;
        
    } catch (SQLException e) {
        logger.error("Failed to update product with ID: " + product.getId(), e);
        return false;
    }
}
```

### Delete Operation
```java
public boolean deleteProduct(int id) {
    String sql = "DELETE FROM products WHERE id = ?";
    
    try (Connection conn = getConnection();
         PreparedStatement stmt = conn.prepareStatement(sql)) {
        
        stmt.setInt(1, id);
        
        int rowsAffected = stmt.executeUpdate();
        return rowsAffected > 0;
        
    } catch (SQLException e) {
        logger.error("Failed to delete product with ID: " + id, e);
        return false;
    }
}
```

## 🔒 Best Practices

### Security
- **Use PreparedStatement**: Prevent SQL injection attacks
- **Input Validation**: Validate all user inputs
- **Connection Security**: Use SSL connections in production
- **User Privileges**: Create dedicated database users with minimal privileges
- **Password Management**: Use environment variables for sensitive data

### Performance
- **Connection Pooling**: Implement connection pooling for better performance
- **Batch Operations**: Use batch processing for multiple operations
- **Proper Indexing**: Create appropriate database indexes
- **Resource Management**: Always close database resources
- **Query Optimization**: Use efficient SQL queries

### Code Quality
- **Separation of Concerns**: Use DAO pattern for data access
- **Error Handling**: Implement comprehensive exception handling
- **Logging**: Add proper logging for debugging and monitoring
- **Documentation**: Document all methods and classes
- **Unit Testing**: Write comprehensive unit tests

## 🧪 Testing Guidelines

### Unit Testing Framework Setup
```java
// Add JUnit 5 dependency for testing
// Maven: junit-jupiter-engine:5.9.2
// Gradle: testImplementation 'org.junit.jupiter:junit-jupiter:5.9.2'

@TestMethodOrder(OrderAnnotation.class)
class ProductDAOTest {
    private static ProductDAO productDAO;
    private static Product testProduct;
    
    @BeforeAll
    static void setUp() {
        productDAO = new ProductDAO();
        testProduct = new Product("Test Product", "Test Description", 99.99, 10, "Test Category");
    }
    
    @Test
    @Order(1)
    void testAddProduct() {
        assertTrue(productDAO.addProduct(testProduct));
        assertNotNull(testProduct.getId());
    }
    
    @Test
    @Order(2)
    void testGetProductById() {
        Product retrieved = productDAO.getProductById(testProduct.getId());
        assertNotNull(retrieved);
        assertEquals(testProduct.getName(), retrieved.getName());
    }
    
    // More test methods...
}
```

### Manual Testing Checklist
- [ ] Database connection successful
- [ ] All CRUD operations work correctly
- [ ] Input validation prevents invalid data
- [ ] Error messages are user-friendly
- [ ] Application handles database disconnection gracefully
- [ ] Memory usage is stable (no leaks)
- [ ] Performance is acceptable with large datasets

## 🚀 Future Enhancements

### Technical Improvements
- **Connection Pooling**: Implement HikariCP or Apache DBCP
- **ORM Integration**: Add Hibernate or MyBatis support
- **RESTful API**: Convert to Spring Boot web application
- **GraphQL Support**: Add GraphQL query capabilities
- **Microservices**: Split into microservices architecture

### Feature Additions
- **Search and Filter**: Advanced product search capabilities
- **Inventory Management**: Stock tracking and alerts
- **User Authentication**: Multi-user support with roles
- **Audit Trail**: Track all data changes
- **Reporting**: Generate sales and inventory reports
- **Import/Export**: CSV, Excel, and JSON data handling

### UI/UX Improvements
- **Desktop GUI**: JavaFX or Swing interface
- **Web Interface**: HTML/CSS/JavaScript frontend
- **Mobile App**: Android/iOS companion app
- **Dashboard**: Real-time analytics dashboard

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### How to Contribute
1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Contribution Guidelines
- Follow Java coding standards and conventions
- Add comprehensive JavaDoc documentation
- Include unit tests for new features
- Update README for significant changes
- Ensure backward compatibility
- Test on multiple operating systems

### Code Review Process
- All submissions require code review
- Maintain test coverage above 80%
- Follow security best practices
- Optimize for performance and memory usage

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### MIT License Summary
- ✅ Commercial use
- ✅ Modification
- ✅ Distribution
- ✅ Private use
- ❌ Liability
- ❌ Warranty

## 👨‍💻 Author

**M Swayam Prakash**
- GitHub: [@Ritish-Sanala](https://github.com/Ritish-Sanala)
- Email: sanalaritish@gmail.com
- Project Link: [https://github.com/Ritish-Sanala/java-crud-mysql](https://github.com/Ritish-Sanala/java-crud-mysql)

---

## 🙏 Acknowledgments

- [Oracle Java Documentation](https://docs.oracle.com/javase/) for comprehensive Java guides
- [MySQL Documentation](https://dev.mysql.com/doc/) for database setup and configuration
- [JDBC Tutorial](https://docs.oracle.com/javase/tutorial/jdbc/) for database connectivity examples
- Stack Overflow community for troubleshooting support

---

## 📞 Support

If you encounter any issues or have questions:

1. **Check the [Common Errors & Solutions](#common-errors--solutions) section**
2. **Search existing [GitHub Issues](https://github.com/Ritish-Sanala/java-crud-mysql/issues)**
3. **Create a new issue** with detailed error information
4. **Contact the author** via email for urgent matters

---

**⭐ If this project helped you learn Java database programming, please give it a star!**

**Happy Coding! 🚀**