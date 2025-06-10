# Node.js Custom Server and Core Modules Exploration

## Project Overview

**Program 8** - A comprehensive exploration of Node.js core modules and custom HTTP server implementation, demonstrating the power of Node.js built-in functionality without external dependencies.


## Objective

To create a custom HTTP server using Node.js and explore the capabilities of core built-in modules including `http`, `os`, `path`, and `events`, demonstrating fundamental server-side programming concepts and system-level operations.

## Key Learning Outcomes

- **HTTP Server Creation**: Build servers without external frameworks
- **System Information Access**: Retrieve OS and hardware details
- **Path Manipulation**: Handle file system paths across platforms
- **Event-Driven Programming**: Implement custom event systems
- **Node.js Core Modules**: Master built-in functionality

## Architecture & Technology Stack

### Runtime Environment
- **Node.js** - JavaScript runtime built on Chrome's V8 engine
- **Built-in HTTP Module** - Native server creation capabilities
- **Core Modules** - No external dependencies required

### Development Environment
- **IDE**: Visual Studio Code with Node.js extensions
- **Version Control**: Git for source code management
- **Testing**: Node.js built-in testing capabilities

## Project Structure

```
node-server-demo/
│
├── src/
│   ├── server.js                       # Custom HTTP server implementation
│   ├── modules/
│   │   ├── osInfo.js                   # Operating system information demo
│   │   ├── pathDemo.js                 # Path manipulation utilities
│   │   └── eventDemo.js                # Event-driven programming example
│   └── utils/
│       ├── serverUtils.js              # Server helper functions
│       └── logger.js                   # Custom logging utility
│
├── public/
│   ├── index.html                      # Static HTML page
│   ├── styles.css                      # CSS styling
│   └── scripts.js                      # Client-side JavaScript
│
├── logs/
│   └── server.log                      # Application logs
│
├── tests/
│   ├── server.test.js                  # Server functionality tests
│   └── modules.test.js                 # Core modules tests
│
├── package.json                        # Project metadata and scripts
├── .gitignore                          # Git ignore patterns
└── README.md                           # Project documentation
```

## Prerequisites

Before setting up the project, ensure you have:

- **Node.js** (version 14.0 or higher) - [Download from nodejs.org](https://nodejs.org/)
- **npm** (comes with Node.js installation)
- **Visual Studio Code** or preferred IDE
- **Git** for version control
- **Web Browser** (Chrome, Firefox, Safari, or Edge)

### Verify Installation
```bash
# Check Node.js version
node --version

# Check npm version
npm --version

# Verify core modules availability
node -e "console.log(Object.keys(require('module').builtinModules))"
```

## Installation & Setup Guide

### Step 1: Project Initialization

```bash
# Create project directory
mkdir node-server-demo
cd node-server-demo

# Initialize npm project
npm init -y

# Create directory structure
mkdir -p src/{modules,utils} public logs tests

# Initialize Git repository
git init
```

### Step 2: Create Package.json Scripts

```json
{
  "name": "node-server-demo",
  "version": "1.0.0",
  "description": "Node.js Custom Server and Core Modules Exploration",
  "main": "src/server.js",
  "scripts": {
    "start": "node src/server.js",
    "dev": "node --watch src/server.js",
    "test": "node tests/server.test.js",
    "os-info": "node src/modules/osInfo.js",
    "path-demo": "node src/modules/pathDemo.js",
    "event-demo": "node src/modules/eventDemo.js",
    "all-demos": "npm run os-info && npm run path-demo && npm run event-demo"
  },
  "keywords": ["nodejs", "http-server", "core-modules", "events"],
  "author": "Ritish Sanala <sanalaritish@gmail.com>",
  "license": "MIT",
  "engines": {
    "node": ">=14.0.0"
  }
}
```

### Step 3: Core Module Implementations

#### HTTP Server (src/server.js)
```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');
const url = require('url');

const PORT = process.env.PORT || 3000;
const HOST = 'localhost';

// Create HTTP server
const server = http.createServer((req, res) => {
    const parsedUrl = url.parse(req.url, true);
    const pathname = parsedUrl.pathname;
    
    // Set CORS headers
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
    
    // Route handling
    switch (pathname) {
        case '/':
            serveStaticFile(res, 'public/index.html', 'text/html');
            break;
        case '/api/system':
            handleSystemInfo(req, res);
            break;
        case '/api/health':
            handleHealthCheck(req, res);
            break;
        default:
            handleNotFound(res);
    }
});

// Serve static files
function serveStaticFile(res, filePath, contentType) {
    fs.readFile(filePath, (err, data) => {
        if (err) {
            res.writeHead(500, { 'Content-Type': 'text/plain' });
            res.end('Internal Server Error');
            return;
        }
        res.writeHead(200, { 'Content-Type': contentType });
        res.end(data);
    });
}

// System information endpoint
function handleSystemInfo(req, res) {
    const os = require('os');
    const systemInfo = {
        platform: os.platform(),
        architecture: os.arch(),
        cpus: os.cpus().length,
        totalMemory: Math.round(os.totalmem() / 1024 / 1024 / 1024) + ' GB',
        freeMemory: Math.round(os.freemem() / 1024 / 1024 / 1024) + ' GB',
        uptime: Math.round(os.uptime() / 3600) + ' hours',
        nodeVersion: process.version
    };
    
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify(systemInfo, null, 2));
}

// Health check endpoint
function handleHealthCheck(req, res) {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ status: 'OK', timestamp: new Date().toISOString() }));
}

// 404 handler
function handleNotFound(res) {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('404 - Page Not Found');
}

// Start server
server.listen(PORT, HOST, () => {
    console.log(`🚀 Server running at http://${HOST}:${PORT}/`);
    console.log(`📊 System info: http://${HOST}:${PORT}/api/system`);
    console.log(`💚 Health check: http://${HOST}:${PORT}/api/health`);
});

// Graceful shutdown
process.on('SIGTERM', () => {
    console.log('📴 Shutting down server gracefully...');
    server.close(() => {
        console.log('✅ Server closed');
        process.exit(0);
    });
});

module.exports = server;
```

## Core Module Demonstrations

### 1. Operating System Module (src/modules/osInfo.js)
```javascript
const os = require('os');

console.log('🖥️  Operating System Information');
console.log('================================');

// Basic system info
console.log(`Platform: ${os.platform()}`);
console.log(`Architecture: ${os.arch()}`);
console.log(`Hostname: ${os.hostname()}`);
console.log(`OS Type: ${os.type()}`);
console.log(`OS Release: ${os.release()}`);

// Memory information
const totalMem = (os.totalmem() / 1024 / 1024 / 1024).toFixed(2);
const freeMem = (os.freemem() / 1024 / 1024 / 1024).toFixed(2);
const usedMem = (totalMem - freeMem).toFixed(2);

console.log(`\n💾 Memory Information:`);
console.log(`Total Memory: ${totalMem} GB`);
console.log(`Free Memory: ${freeMem} GB`);
console.log(`Used Memory: ${usedMem} GB`);

// CPU information
console.log(`\n🔥 CPU Information:`);
console.log(`CPU Cores: ${os.cpus().length}`);
console.log(`CPU Model: ${os.cpus()[0].model}`);

// System uptime
const uptime = os.uptime();
const hours = Math.floor(uptime / 3600);
const minutes = Math.floor((uptime % 3600) / 60);
console.log(`\n⏱️  System Uptime: ${hours}h ${minutes}m`);

// User information
console.log(`\n👤 User Information:`);
console.log(`Username: ${os.userInfo().username}`);
console.log(`Home Directory: ${os.userInfo().homedir}`);

// Network interfaces
console.log(`\n🌐 Network Interfaces:`);
const networkInterfaces = os.networkInterfaces();
Object.keys(networkInterfaces).forEach(interface => {
    console.log(`${interface}:`);
    networkInterfaces[interface].forEach(details => {
        if (details.family === 'IPv4') {
            console.log(`  - ${details.address} (${details.internal ? 'internal' : 'external'})`);
        }
    });
});
```

### 2. Path Module (src/modules/pathDemo.js)
```javascript
const path = require('path');

console.log('📁 Path Module Demonstration');
console.log('============================');

// Sample file paths
const filePath = '/users/ritish/documents/project/index.js';
const windowsPath = 'C:\\Users\\Ritish\\Documents\\project\\index.js';

console.log('🔍 Path Analysis:');
console.log(`Original path: ${filePath}`);
console.log(`Directory: ${path.dirname(filePath)}`);
console.log(`Filename: ${path.basename(filePath)}`);
console.log(`Extension: ${path.extname(filePath)}`);
console.log(`Filename without extension: ${path.basename(filePath, path.extname(filePath))}`);

console.log('\n🔧 Path Operations:');
// Path joining
const joinedPath = path.join('/users', 'ritish', 'documents', 'project', 'index.js');
console.log(`Joined path: ${joinedPath}`);

// Path resolution
const resolvedPath = path.resolve('src', 'modules', 'pathDemo.js');
console.log(`Resolved path: ${resolvedPath}`);

// Path normalization
const messyPath = '/users/ritish/../ritish/./documents//project/index.js';
console.log(`Messy path: ${messyPath}`);
console.log(`Normalized: ${path.normalize(messyPath)}`);

// Cross-platform compatibility
console.log('\n🌍 Cross-Platform Paths:');
console.log(`Current separator: "${path.sep}"`);
console.log(`Current delimiter: "${path.delimiter}"`);
console.log(`POSIX separator: "${path.posix.sep}"`);
console.log(`Windows separator: "${path.win32.sep}"`);

// Parse path object
console.log('\n📋 Path Object:');
const pathObj = path.parse(filePath);
console.log(JSON.stringify(pathObj, null, 2));

// Format path from object
const newPathObj = {
    dir: '/users/ritish/projects',
    name: 'app',
    ext: '.js'
};
console.log(`Formatted path: ${path.format(newPathObj)}`);
```

### 3. Events Module (src/modules/eventDemo.js)
```javascript
const EventEmitter = require('events');

console.log('🎭 Events Module Demonstration');
console.log('==============================');

// Create custom event emitter
class CustomEmitter extends EventEmitter {}
const myEmitter = new CustomEmitter();

// Event listeners
myEmitter.on('greeting', (name) => {
    console.log(`👋 Hello, ${name}!`);
});

myEmitter.on('greeting', (name) => {
    console.log(`🎉 Welcome to our application, ${name}!`);
});

// One-time event listener
myEmitter.once('firstTime', (user) => {
    console.log(`🌟 First time visitor: ${user}`);
});

// Error event handler
myEmitter.on('error', (err) => {
    console.error(`❌ Error occurred: ${err.message}`);
});

// Emit events
console.log('\n📢 Emitting Events:');
myEmitter.emit('greeting', 'Alice');
myEmitter.emit('greeting', 'Bob');

myEmitter.emit('firstTime', 'Charlie');
myEmitter.emit('firstTime', 'Diana'); // This won't trigger (once listener)

// Emit error
myEmitter.emit('error', new Error('Something went wrong'));

// Event listener management
console.log('\n📊 Event Listener Stats:');
console.log(`Greeting listeners: ${myEmitter.listenerCount('greeting')}`);
console.log(`FirstTime listeners: ${myEmitter.listenerCount('firstTime')}`);

// Remove specific listener
const specificHandler = (name) => console.log(`Specific handler for ${name}`);
myEmitter.on('test', specificHandler);
console.log(`Test listeners before removal: ${myEmitter.listenerCount('test')}`);
myEmitter.removeListener('test', specificHandler);
console.log(`Test listeners after removal: ${myEmitter.listenerCount('test')}`);

// Custom event class example
class UserManager extends EventEmitter {
    createUser(userData) {
        // Simulate user creation
        console.log(`👤 Creating user: ${userData.name}`);
        
        // Emit user created event
        this.emit('userCreated', userData);
        
        // Emit notification event
        this.emit('notification', {
            type: 'success',
            message: `User ${userData.name} created successfully`
        });
    }
}

const userManager = new UserManager();

userManager.on('userCreated', (user) => {
    console.log(`✅ User created event received: ${user.name} (${user.email})`);
});

userManager.on('notification', (notification) => {
    console.log(`🔔 ${notification.type.toUpperCase()}: ${notification.message}`);
});

// Trigger custom events
console.log('\n🎯 Custom Event System:');
userManager.createUser({ name: 'Ritish', email: 'sanalaritish@gmail.com' });
```

## Common Issues & Solutions

### Issue 1: Port Already in Use
**Error**: `EADDRINUSE: address already in use :::3000`

**Solution**:
```bash
# Find process using port 3000
lsof -i :3000        # macOS/Linux
netstat -ano | findstr :3000   # Windows

# Kill the process
kill -9 <PID>        # macOS/Linux
taskkill /PID <PID> /F         # Windows

# Or use different port
PORT=3001 node src/server.js
```

**Prevention**:
```javascript
// In server.js - Add error handling
server.on('error', (err) => {
    if (err.code === 'EADDRINUSE') {
        console.log(`❌ Port ${PORT} is already in use`);
        console.log('🔄 Trying alternative port...');
        server.listen(PORT + 1, HOST);
    }
});
```

### Issue 2: Module Not Found Error
**Error**: `Cannot find module 'xyz'`

**Solution**:
```javascript
// Check if module exists before requiring
try {
    const requiredModule = require('xyz');
} catch (err) {
    if (err.code === 'MODULE_NOT_FOUND') {
        console.error('Module xyz not found. Installing...');
        // Handle gracefully or provide fallback
    }
}
```

**Root Cause**: Using external modules instead of core modules, or incorrect path.

### Issue 3: File System Permission Errors
**Error**: `EACCES: permission denied`

**Solution**:
```javascript
// Check file permissions before reading
const fs = require('fs');

fs.access('filename.txt', fs.constants.R_OK, (err) => {
    if (err) {
        console.error('File is not readable');
    } else {
        // Proceed with file operations
    }
});
```

**Alternative**:
```bash
# Fix file permissions
chmod 644 filename.txt        # Make file readable
chmod 755 directory/          # Make directory accessible
```

### Issue 4: Event Memory Leaks
**Error**: `MaxListenersExceededWarning: Possible EventEmitter memory leak detected`

**Solution**:
```javascript
// Increase max listeners if needed
myEmitter.setMaxListeners(20);

// Or remove listeners when done
myEmitter.removeAllListeners('eventName');

// Properly clean up in long-running applications
process.on('exit', () => {
    myEmitter.removeAllListeners();
});
```

### Issue 5: Path Resolution Issues on Windows
**Error**: Path operations failing on Windows vs Unix systems

**Solution**:
```javascript
const path = require('path');

// Always use path.join instead of string concatenation
const filePath = path.join(__dirname, 'public', 'index.html');

// Use path.resolve for absolute paths
const absolutePath = path.resolve(process.cwd(), 'src', 'server.js');

// Handle both separators
const normalizedPath = path.normalize(userPath);
```

### Issue 6: Server Not Responding
**Error**: Server starts but doesn't respond to requests

**Solution**:
```javascript
// Add request logging
const server = http.createServer((req, res) => {
    console.log(`📥 ${new Date().toISOString()} - ${req.method} ${req.url}`);
    
    // Add timeout handling
    req.setTimeout(5000, () => {
        res.writeHead(408, { 'Content-Type': 'text/plain' });
        res.end('Request Timeout');
    });
    
    // Your route handling here
});

// Add server error handling
server.on('error', (err) => {
    console.error('Server error:', err);
});
```

### Issue 7: JSON Parsing Errors
**Error**: `SyntaxError: Unexpected token in JSON`

**Solution**:
```javascript
// Safe JSON parsing
function safeJsonParse(str) {
    try {
        return JSON.parse(str);
    } catch (err) {
        console.error('JSON parse error:', err.message);
        return null;
    }
}

// Handle POST data properly
let body = '';
req.on('data', chunk => {
    body += chunk.toString();
});

req.on('end', () => {
    const jsonData = safeJsonParse(body);
    if (jsonData) {
        // Process valid JSON
    } else {
        res.writeHead(400, { 'Content-Type': 'text/plain' });
        res.end('Invalid JSON');
    }
});
```

## Running the Project

### Individual Module Testing
```bash
# Test HTTP server
npm start
# or
node src/server.js

# Test OS information
npm run os-info
# or
node src/modules/osInfo.js

# Test path operations
npm run path-demo
# or
node src/modules/pathDemo.js

# Test event system
npm run event-demo
# or
node src/modules/eventDemo.js

# Run all demos
npm run all-demos
```

### Development Mode with Auto-Restart
```bash
# Install nodemon for development (optional)
npm install -g nodemon

# Run with auto-restart
nodemon src/server.js

# Or use Node.js built-in watch mode (Node 18+)
npm run dev
```

## Expected Output Examples

### Server Output
```
🚀 Server running at http://localhost:3000/
📊 System info: http://localhost:3000/api/system
💚 Health check: http://localhost:3000/api/health
```

### OS Info Output
```
🖥️  Operating System Information
================================
Platform: darwin
Architecture: x64
Hostname: Ritishs-MacBook
OS Type: Darwin
OS Release: 21.6.0

💾 Memory Information:
Total Memory: 16.00 GB
Free Memory: 8.45 GB
Used Memory: 7.55 GB
```

### Path Demo Output
```
📁 Path Module Demonstration
============================
🔍 Path Analysis:
Original path: /users/ritish/documents/project/index.js
Directory: /users/ritish/documents/project
Filename: index.js
Extension: .js
```

### Event Demo Output
```
🎭 Events Module Demonstration
==============================
📢 Emitting Events:
👋 Hello, Alice!
🎉 Welcome to our application, Alice!
👋 Hello, Bob!
🎉 Welcome to our application, Bob!
🌟 First time visitor: Charlie
```

## API Endpoints

| Method | Endpoint | Description | Response |
|--------|----------|-------------|----------|
| GET | `/` | Home page | HTML page |
| GET | `/api/system` | System information | JSON system data |
| GET | `/api/health` | Health check | JSON status |

## Testing

### Manual Testing
```bash
# Test server endpoints
curl http://localhost:3000/
curl http://localhost:3000/api/system
curl http://localhost:3000/api/health
```

### Automated Testing
```javascript
// tests/server.test.js
const http = require('http');
const server = require('../src/server');

function testEndpoint(path, expectedStatus) {
    return new Promise((resolve, reject) => {
        const req = http.request({
            hostname: 'localhost',
            port: 3000,
            path: path,
            method: 'GET'
        }, (res) => {
            if (res.statusCode === expectedStatus) {
                resolve(true);
            } else {
                reject(new Error(`Expected ${expectedStatus}, got ${res.statusCode}`));
            }
        });
        
        req.on('error', reject);
        req.end();
    });
}

// Run tests
async function runTests() {
    try {
        await testEndpoint('/', 200);
        await testEndpoint('/api/system', 200);
        await testEndpoint('/api/health', 200);
        await testEndpoint('/nonexistent', 404);
        console.log('✅ All tests passed');
    } catch (error) {
        console.error('❌ Test failed:', error.message);
    }
}
```

## Performance Considerations

### Memory Usage
```javascript
// Monitor memory usage
setInterval(() => {
    const used = process.memoryUsage();
    console.log('Memory usage:', {
        rss: Math.round(used.rss / 1024 / 1024) + ' MB',
        heapTotal: Math.round(used.heapTotal / 1024 / 1024) + ' MB',
        heapUsed: Math.round(used.heapUsed / 1024 / 1024) + ' MB'
    });
}, 10000);
```

### Event Loop Monitoring
```javascript
// Monitor event loop lag
function measureEventLoopLag() {
    const start = process.hrtime.bigint();
    setImmediate(() => {
        const lag = process.hrtime.bigint() - start;
        console.log(`Event loop lag: ${Number(lag) / 1000000}ms`);
    });
}
```

## Security Considerations

### Basic Security Headers
```javascript
function setSecurityHeaders(res) {
    res.setHeader('X-Content-Type-Options', 'nosniff');
    res.setHeader('X-Frame-Options', 'DENY');
    res.setHeader('X-XSS-Protection', '1; mode=block');
    res.setHeader('Strict-Transport-Security', 'max-age=31536000');
}
```

### Input Validation
```javascript
function validateInput(input) {
    if (typeof input !== 'string') return false;
    if (input.length > 1000) return false;
    if (/<script|javascript:/i.test(input)) return false;
    return true;
}
```

## Future Enhancements

- [ ] Add HTTPS support with SSL certificates
- [ ] Implement request rate limiting
- [ ] Add comprehensive logging system
- [ ] Create REST API with full CRUD operations
- [ ] Add WebSocket support for real-time features
- [ ] Implement clustering for multi-core utilization
- [ ] Add unit and integration test suites
- [ ] Create Docker containerization

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/enhancement`)
3. Commit your changes (`git commit -am 'Add enhancement'`)
4. Push to the branch (`git push origin feature/enhancement`)
5. Create a Pull Request

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For questions, suggestions, or collaboration opportunities:

- **Email**: sanalaritish@gmail.com
- **GitHub**: [@Ritish-Sanala](https://github.com/Ritish-Sanala)

---

*This project demonstrates the power and flexibility of Node.js core modules, providing a foundation for understanding server-side JavaScript development without external dependencies.*