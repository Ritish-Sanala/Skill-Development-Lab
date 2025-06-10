# Shopping Cart Session Management Web Application

## Project Overview

**Program 7** - A comprehensive Java Servlet-based shopping cart application demonstrating HTTP session management for maintaining user-specific data across multiple requests without database persistence for every interaction.


## Objective

To implement robust session tracking in a shopping cart web application using HTTP Sessions in Java Servlets, demonstrating how to maintain user state and improve application performance through in-memory session storage.

## Key Features

- **Session-Based Cart Management**: Maintains cart data across page requests
- **Multi-User Support**: Each user has their own isolated shopping cart
- **Performance Optimization**: Reduces database writes by using session storage
- **Stateful Web Application**: Demonstrates HTTP session management
- **Real-time Cart Updates**: Immediate feedback without page refresh

## Architecture & Technology Stack

### Backend Technologies
- **Java Servlets** - Controller layer with session management
- **HTTP Sessions** - User state management
- **MySQL Database** - Product data persistence
- **Apache Tomcat** - Servlet container and session management

### Frontend Technologies
- **HTML5** - Structure and semantic markup
- **CSS3** - Styling and responsive design
- **JavaScript** - Client-side interactivity
- **AJAX** - Asynchronous cart updates

### Development Environment
- **IDE**: Visual Studio Code with Java Extension Pack
- **Build System**: Manual compilation with javac
- **Database**: MySQL Server with JDBC connectivity

## Project Structure

```
shopping-cart-session/
│
├── src/
│   └── servlets/
│       ├── ProductServlet.java         # Product listing and management
│       └── CartServlet.java            # Cart operations with session handling
│
├── WebContent/
│   ├── index.html                      # Welcome/landing page
│   ├── catalog.html                    # Product catalog with add-to-cart
│   ├── cart.html                       # Cart view with session data
│   ├── css/
│   │   ├── styles.css                  # Main stylesheet
│   │   └── cart.css                    # Cart-specific styles
│   ├── js/
│   │   ├── cart.js                     # Cart management functions
│   │   └── session.js                  # Session handling utilities
│   └── WEB-INF/
│       ├── web.xml                     # Servlet and session configuration
│       ├── classes/                    # Compiled servlet classes
│       └── lib/
│           └── mysql-connector-j-9.3.0.jar
│
├── bin/                                # Compiled .class files
├── lib/
│   └── javax.servlet-api-4.0.1.jar    # Servlet API dependency
├── logs/                               # Application logs
└── README.md                           # Project documentation
```

## Prerequisites

Ensure the following software is installed before setting up the project:

- **Java JDK 8 or higher** (recommended: JDK 11+)
- **Apache Tomcat 9.0+** (with session management enabled)
- **MySQL Server 8.0+**
- **Visual Studio Code** with Java Extension Pack
- **MySQL Workbench** (optional, for database management)
- **Web Browser** (Chrome, Firefox, or Safari)

## Installation & Setup Guide

### Step 1: Database Configuration

1. **Create Database Schema**:
```sql
CREATE DATABASE shopping_cart;
USE shopping_cart;

-- Products table
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    description TEXT,
    image_url VARCHAR(255),
    stock_quantity INT DEFAULT 0,
    category VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Insert sample products
INSERT INTO products (name, price, description, image_url, stock_quantity, category) VALUES
('Gaming Laptop', 1299.99, 'High-performance gaming laptop with RTX graphics', '/images/laptop.jpg', 15, 'Electronics'),
('Wireless Mouse', 29.99, 'Ergonomic wireless mouse with precision tracking', '/images/mouse.jpg', 50, 'Accessories'),
('Mechanical Keyboard', 149.99, 'RGB mechanical keyboard with blue switches', '/images/keyboard.jpg', 25, 'Accessories'),
('4K Monitor', 399.99, '27-inch 4K IPS monitor with USB-C', '/images/monitor.jpg', 12, 'Electronics'),
('Bluetooth Headphones', 199.99, 'Noise-canceling wireless headphones', '/images/headphones.jpg', 30, 'Audio'),
('Smartphone', 799.99, 'Latest flagship smartphone with AI camera', '/images/phone.jpg', 20, 'Electronics');

-- Optional: Create users table for future authentication
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Step 2: Project Setup

1. **Download Required Dependencies**:
   ```bash
   # Create lib directory if it doesn't exist
   mkdir -p lib
   
   # Download MySQL Connector (or copy from your MySQL installation)
   # Download Servlet API JAR
   ```

2. **Organize JAR Files**:
   ```bash
   # For compilation
   cp mysql-connector-j-9.3.0.jar lib/
   cp javax.servlet-api-4.0.1.jar lib/
   
   # For runtime (Tomcat deployment)
   cp mysql-connector-j-9.3.0.jar WebContent/WEB-INF/lib/
   ```

### Step 3: Compilation

```bash
# Navigate to project root
cd shopping-cart-session/

# Compile all servlets
javac -cp "lib/*" -d bin src/servlets/*.java

# Verify compilation successful
ls -la bin/servlets/
# Should show: CartServlet.class, ProductServlet.class
```

### Step 4: Deployment Configuration

1. **Configure Session Management in web.xml**:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <display-name>Shopping Cart Session Management</display-name>
    
    <!-- Session Configuration -->
    <session-config>
        <session-timeout>30</session-timeout> <!-- 30 minutes -->
        <cookie-config>
            <http-only>true</http-only>
            <secure>false</secure> <!-- Set to true for HTTPS -->
        </cookie-config>
        <tracking-mode>COOKIE</tracking-mode>
    </session-config>
    
    <!-- Product Servlet -->
    <servlet>
        <servlet-name>ProductServlet</servlet-name>
        <servlet-class>servlets.ProductServlet</servlet-class>
        <init-param>
            <param-name>db-url</param-name>
            <param-value>jdbc:mysql://localhost:3306/shopping_cart</param-value>
        </init-param>
    </servlet>
    
    <!-- Cart Servlet -->
    <servlet>
        <servlet-name>CartServlet</servlet-name>
        <servlet-class>servlets.CartServlet</servlet-class>
    </servlet>
    
    <!-- Servlet Mappings -->
    <servlet-mapping>
        <servlet-name>ProductServlet</servlet-name>
        <url-pattern>/products</url-pattern>
    </servlet-mapping>
    
    <servlet-mapping>
        <servlet-name>CartServlet</servlet-name>
        <url-pattern>/cart</url-pattern>
    </servlet-mapping>
    
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>
</web-app>
```

### Step 5: Deployment to Tomcat

```bash
# Method 1: Direct deployment
cp -r WebContent/ $CATALINA_HOME/webapps/shopping-cart-session/
cp -r bin/* $CATALINA_HOME/webapps/shopping-cart-session/WEB-INF/classes/

# Method 2: Create WAR file (recommended for production)
jar -cvf shopping-cart-session.war -C WebContent/ .
cp shopping-cart-session.war $CATALINA_HOME/webapps/
```

### Step 6: Start and Test

```bash
# Start Tomcat
$CATALINA_HOME/bin/startup.sh    # Linux/Mac
$CATALINA_HOME/bin/startup.bat   # Windows

# Access application
# Home Page: http://localhost:8080/shopping-cart-session/
# Products: http://localhost:8080/shopping-cart-session/products
# Cart: http://localhost:8080/shopping-cart-session/cart
```

## Session Management Implementation

### Session Storage Strategy
- **Cart Data**: Stored as `List<CartItem>` in session
- **User Preferences**: Stored as session attributes
- **Session Timeout**: Configurable (default: 30 minutes)
- **Session ID**: Automatically managed by Tomcat

### Cart Item Structure
```java
public class CartItem {
    private int productId;
    private String productName;
    private double price;
    private int quantity;
    private double totalPrice;
    // getters and setters
}
```

## API Endpoints

| Method | Endpoint | Description | Parameters | Response |
|--------|----------|-------------|------------|----------|
| GET | `/products` | List all products | None | JSON array of products |
| GET | `/products?id={id}` | Get specific product | `id` | JSON product object |
| POST | `/cart` | Add item to cart | `productId`, `quantity` | JSON success/error |
| GET | `/cart` | Get cart contents | None | JSON cart items |
| PUT | `/cart` | Update cart item | `productId`, `quantity` | JSON updated cart |
| DELETE | `/cart` | Remove from cart | `productId` | JSON updated cart |
| POST | `/cart/clear` | Clear entire cart | None | JSON success message |

## Common Issues & Solutions

### Issue 1: Session Not Persisting
**Error**: Cart items disappear after page refresh or navigation

**Solution**:
```java
// In CartServlet.java - Ensure proper session retrieval
HttpSession session = request.getSession(true); // Create if doesn't exist
List<CartItem> cart = (List<CartItem>) session.getAttribute("cart");
if (cart == null) {
    cart = new ArrayList<>();
    session.setAttribute("cart", cart);
}
```

**Root Cause**: Not creating session properly or session timeout too short.

### Issue 2: ClassCastException with Session Attributes
**Error**: `java.lang.ClassCastException: java.util.ArrayList cannot be cast to CartItem`

**Solution**:
```java
// Proper type checking and casting
Object cartObj = session.getAttribute("cart");
List<CartItem> cart;
if (cartObj instanceof List) {
    cart = (List<CartItem>) cartObj;
} else {
    cart = new ArrayList<>();
    session.setAttribute("cart", cart);
}
```

### Issue 3: Session Timeout Issues
**Error**: Cart data lost unexpectedly

**Solution**:
```xml
<!-- In web.xml - Increase session timeout -->
<session-config>
    <session-timeout>60</session-timeout> <!-- 60 minutes -->
</session-config>
```

**Additional Fix**:
```java
// In servlet - Check session validity
if (request.getSession(false) == null) {
    response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "Session expired");
    return;
}
```

### Issue 4: Multiple Browser Tabs Sharing Cart
**Error**: Different tabs showing same cart (expected behavior, but sometimes undesired)

**Solution**:
```java
// Option 1: Use session-per-tab (store tab ID)
String tabId = request.getParameter("tabId");
String cartKey = "cart_" + tabId;

// Option 2: Use URL rewriting for session tracking
String encodedURL = response.encodeURL("catalog.html");
```

### Issue 5: Memory Leaks with Sessions
**Error**: Server memory usage keeps increasing

**Solution**:
```java
// Implement session cleanup
public class SessionCleanupListener implements HttpSessionListener {
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        session.removeAttribute("cart");
        // Clean up other session attributes
    }
}
```

### Issue 6: JSESSIONID Cookie Issues
**Error**: Session not maintained across requests

**Solution**:
```java
// Enable URL rewriting as fallback
String url = response.encodeURL("cart.html");

// Or configure cookie settings in web.xml
```

### Issue 7: Concurrent Session Modification
**Error**: Cart gets corrupted with multiple rapid requests

**Solution**:
```java
// Synchronize session access
synchronized(session) {
    List<CartItem> cart = (List<CartItem>) session.getAttribute("cart");
    // Modify cart safely
    session.setAttribute("cart", cart);
}
```

## Testing Guidelines

### Manual Testing Scenarios

1. **Session Creation Test**:
   - Open browser, navigate to application
   - Add item to cart
   - Verify session cookie is created
   - Check cart persistence across page navigation

2. **Multi-Tab Test**:
   - Open multiple tabs
   - Add different items in each tab
   - Verify cart is shared across tabs

3. **Session Timeout Test**:
   - Add items to cart
   - Wait for session timeout
   - Try to access cart - should be empty

4. **Concurrent User Test**:
   - Open application in different browsers
   - Verify each has independent cart

### Automated Testing
```bash
# Using curl to test API endpoints
curl -c cookies.txt http://localhost:8080/shopping-cart-session/products
curl -b cookies.txt -X POST http://localhost:8080/shopping-cart-session/cart \
     -d "productId=1&quantity=2"
curl -b cookies.txt http://localhost:8080/shopping-cart-session/cart
```

## Performance Considerations

### Session Optimization
- **Session Size**: Keep cart data minimal
- **Session Timeout**: Balance between user experience and memory usage
- **Session Clustering**: For load-balanced environments
- **Session Persistence**: Configure for high availability

### Memory Management
```java
// Implement session size limits
private static final int MAX_CART_ITEMS = 50;
private static final int MAX_SESSION_SIZE = 1024 * 10; // 10KB
```

## Security Considerations

### Session Security
- **HTTPS Only**: For production environments
- **Session Fixation**: Regenerate session ID after login
- **CSRF Protection**: Implement CSRF tokens
- **XSS Prevention**: Sanitize all user inputs

```java
// Session security implementation
public void doPost(HttpServletRequest request, HttpServletResponse response) {
    // Regenerate session ID for security
    request.getSession().invalidate();
    HttpSession newSession = request.getSession(true);
    
    // Continue with cart operations
}
```

## Future Enhancements

- [ ] Implement user authentication with session management
- [ ] Add cart persistence to database with session backup
- [ ] Implement shopping cart abandonment recovery
- [ ] Add real-time cart sharing between devices
- [ ] Implement cart analytics and tracking
- [ ] Add cart export/import functionality
- [ ] Implement wishlist with session storage

## Troubleshooting Logs

### Enable Debug Logging
```java
// In servlet code
System.out.println("Session ID: " + request.getSession().getId());
System.out.println("Cart Size: " + cart.size());

// Or use proper logging
Logger logger = Logger.getLogger(CartServlet.class.getName());
logger.info("Cart operation: " + operation + " for session: " + sessionId);
```

### Check Tomcat Logs
```bash
# Monitor Tomcat logs for session-related errors
tail -f $CATALINA_HOME/logs/catalina.out
tail -f $CATALINA_HOME/logs/localhost.log
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/session-enhancement`)
3. Commit your changes (`git commit -am 'Add session enhancement'`)
4. Push to the branch (`git push origin feature/session-enhancement`)
5. Create a Pull Request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For questions, suggestions, or collaboration opportunities:

- **Email**: sanalaritish@gmail.com
- **GitHub**: [@Ritish-Sanala](https://github.com/Ritish-Sanala)

---

*This project demonstrates advanced HTTP session management techniques in Java web applications, showcasing best practices for maintaining user state and building scalable web applications.*