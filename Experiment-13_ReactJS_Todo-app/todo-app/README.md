# React TODO App


## AIM
To develop a TODO list application frontend using React, allowing users to add, complete, and delete tasks with a clean Canva-style UI.

## DESCRIPTION
This experiment demonstrates how to build a single-page TODO application using React.js. The app allows users to manage their daily tasks in a dynamic and interactive way. It showcases React concepts like state management using useState, component-based architecture, and basic form handling. This project is ideal for beginners to learn about React components, props, state management, conditional rendering, and basic CSS styling.

## PROJECT STRUCTURE
```
todo-app/
│
├── public/
│   └── index.html          # HTML template
│
├── src/
│   ├── components/
│   │   ├── TodoForm.js     # Component to add new tasks
│   │   ├── TodoItem.js     # Component to render a single task
│   │   └── TodoList.js     # Component to list all tasks
│   ├── App.js              # Main application component
│   ├── App.css             # Styling for the app (Canva style)
│   └── index.js            # Entry point of the React app
│
├── package.json            # Project metadata and dependencies
└── README.md               # Project documentation
```

## INSTALLATION & SETUP

### PREREQUISITES
• Node.js & npm (version 14 or higher recommended)  
• VS Code or any code editor  
• Basic knowledge of React  

### STEPS TO RUN THE PROJECT

1. **Create Project using Create React App**
   ```bash
   npx create-react-app todo-app
   cd todo-app
   ```

2. **Create Components**
   Inside `src/components/`, create the following files:
   • `TodoForm.js`
   • `TodoItem.js`
   • `TodoList.js`
   
   Each should return respective JSX as defined in the project.

3. **Add Logic in App.js**
   Implement useState for managing TODOs, and pass methods for adding, toggling, and removing tasks.

4. **Style the App**
   Use `App.css` to add modern, Canva-style UI.

5. **Run the Project**
   ```bash
   npm start
   ```

6. **Visit in Browser**
   Open your browser and go to: `http://localhost:3000`

## COMMON ERRORS ENCOUNTERED & SOLUTIONS

### Error 1: Module Not Found - Can't resolve './components/TodoForm'
**Problem:** React couldn't find the component files in the components folder.

**Solution:** 
- Ensure the `components` folder exists inside `src/`
- Check file names match exactly (case-sensitive)
- Verify import statements use correct relative paths:
  ```javascript
  import TodoForm from './components/TodoForm';
  ```

### Error 2: useState is not defined
**Problem:** Forgot to import useState hook from React.

**Solution:**
```javascript
import React, { useState } from 'react';
```

### Error 3: Map function key prop warning
**Problem:** Warning about missing "key" prop when rendering todo lists.

**Solution:**
```javascript
{todos.map((todo) => (
  <TodoItem key={todo.id} todo={todo} />
))}
```

### Error 4: Port 3000 already in use
**Problem:** Another application was running on port 3000.

**Solution:**
- Kill the existing process: `Ctrl + C` in terminal
- Or run on different port: `PORT=3001 npm start`

### Error 5: CSS styles not applying
**Problem:** Canva-style CSS wasn't loading properly.

**Solution:**
- Ensure `App.css` is imported in `App.js`:
  ```javascript
  import './App.css';
  ```
- Check CSS selector specificity and syntax

## FEATURES IMPLEMENTED
- ✅ Add new todos
- ✅ Mark todos as complete/incomplete
- ✅ Delete todos
- ✅ Clean, modern UI design
- ✅ Responsive layout
- ✅ Local state management

## TECHNOLOGIES USED
- React.js
- JavaScript (ES6+)
- CSS3
- HTML5

## FUTURE ENHANCEMENTS
- Add local storage persistence
- Implement todo categories
- Add due dates and priorities
- Dark mode toggle

## LICENSE
This project is licensed under the MIT License.

---
*Developed as part of React learning journey by Ritish Sanala*