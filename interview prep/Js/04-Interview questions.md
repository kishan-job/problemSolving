# Common JavaScript Interview Questions

## Frontend Questions

```
1. Fetch data from API and display in table
2. Login form validation
3. Authentication & Protected routes
4. Store data in localStorage/sessionStorage
5. Todo list with CRUD operations
6. Search/filter functionality
7. Pagination
8. Form validation (email, password, phone)
9. File upload
10. Infinite scroll
11. Debounce/Throttle implementation
12. Modal/Dialog implementation
13. Dropdown menu
14. Image lazy loading
15. Dark mode toggle
```

---

## Authentication Questions

### 1. Implement Login Form
- Validate credentials
- Store token in localStorage/cookies
- Redirect to dashboard
- Show only if logged in

### 2. Protected Routes
- Check if user logged in
- If not → redirect to login
- If yes → show page

### 3. Logout Functionality
- Clear token/cookie
- Redirect to login

### 4. Remember Me Checkbox
- Store login data
- Auto-fill on next visit

### 5. JWT Token Handling
- Send token in header
- Refresh token logic
- Handle expired token

---

## API Questions

### 1. Fetch User List from API
- Display in table
- Add pagination
- Add search

### 2. Fetch and Display Posts
- Load more button
- Comments on each post

### 3. Real-time Updates
- Fetch data periodically
- Update UI without refresh

### 4. Error Handling
- Show error message if API fails
- Retry logic

---

## Data Storage Questions

### 1. Store Form Data in localStorage
- Save on input change
- Restore on page reload

### 2. Store User Preferences
- Dark mode
- Language
- Theme

### 3. Cache API Data
- Avoid repeated API calls
- Show cached data first

### 4. Shopping Cart
- Store items in localStorage
- Persist across page refreshes

---

## Form Questions

### 1. Multi-step Form
- Validate each step
- Save progress
- Show summary

### 2. Dynamic Form Fields
- Add/remove fields
- Validation for each

### 3. Auto-save Form
- Save as user types
- Show saved indicator

### 4. Form Reset
- Clear all fields
- Reset to initial state

---

## UI Component Questions

### 1. Accordion
- Expand/collapse sections
- Only one open at a time

### 2. Tabs
- Switch between tabs
- Show/hide content

### 3. Carousel
- Next/prev buttons
- Auto-rotate

### 4. Rating System
- Star rating
- Submit rating

### 5. Notification/Toast
- Show message
- Auto-hide after 3 seconds

---

## Performance Questions

### 1. Optimize API Calls
- Debounce search input
- Cancel previous requests

### 2. Lazy Load Images
- Load on scroll

### 3. Pagination vs Infinite Scroll
- When to use which

### 4. Memoization
- Avoid re-computing

---

## Universal Pattern for ALL Questions

```javascript
// 1. Setup listener
element.addEventListener('event', (e) => {

  // 2. Get data
  const data = e.target.value;

  // 3. Validate/Process
  if (validate(data)) {

    // 4. API call or localStorage
    fetch('/api/endpoint', { method: 'POST', body: data })
      .then(res => res.json())
      .then(data => {

        // 5. Update UI
        updateDOM(data);
        
        // 6. Store if needed
        localStorage.setItem('key', JSON.stringify(data));
      })
      .catch(err => {
        // 7. Handle error
        console.error(err);
        showErrorMessage(err.message);
      });
  }
});
```

---

## Interview Tips

### Master This Pattern
Most questions follow the **same pattern**:

1. **Get user input** - from form/input
2. **Validate it** - check if valid
3. **API call** - fetch data
4. **Handle response** - check for errors
5. **Update DOM** - show data to user
6. **Store if needed** - localStorage/database

### Example: Login Form

```javascript
// 1. Get input
const username = document.getElementById('username').value;
const password = document.getElementById('password').value;

// 2. Validate
if (!username || !password) {
  showError('All fields required');
  return;
}

// 3. API call
fetch('/api/login', {
  method: 'POST',
  body: JSON.stringify({ username, password })
})
.then(res => res.json())
.then(data => {
  // 4. Handle response
  if (data.token) {
    // 5. Store token
    localStorage.setItem('token', data.token);
    
    // 6. Update UI
    window.location.href = '/dashboard';
  }
})
.catch(err => {
  // 7. Handle error
  showError(err.message);
});
```

### Example: Todo CRUD

```javascript
// CREATE
function addTodo(title) {
  fetch('/api/todos', {
    method: 'POST',
    body: JSON.stringify({ title })
  })
  .then(res => res.json())
  .then(todo => renderTodo(todo));
}

// READ
function getTodos() {
  fetch('/api/todos')
  .then(res => res.json())
  .then(todos => renderTodos(todos));
}

// UPDATE
function updateTodo(id, title) {
  fetch(`/api/todos/${id}`, {
    method: 'PUT',
    body: JSON.stringify({ title })
  })
  .then(res => res.json())
  .then(todo => updateDOM(todo));
}

// DELETE
function deleteTodo(id) {
  fetch(`/api/todos/${id}`, { method: 'DELETE' })
  .then(() => removeFromDOM(id));
}
```

---

## What Interviewers Look For

✅ **You should show:**
- Understanding of async/await or .then()
- Error handling
- Input validation
- DOM manipulation
- localStorage usage
- API integration
- Clean code structure

❌ **Avoid:**
- No error handling
- No validation
- Hardcoded values
- Messy code structure
- No comments

---

## Quick Reference

### Get Data
```javascript
const value = element.value;
const data = JSON.parse(localStorage.getItem('key'));
```

### API Call
```javascript
fetch('/api/endpoint', { method: 'POST', body: JSON.stringify(data) })
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

### Update DOM
```javascript
element.innerHTML = data;
element.textContent = data;
element.classList.add('active');
```

### Store Data
```javascript
localStorage.setItem('key', JSON.stringify(data));
const data = JSON.parse(localStorage.getItem('key'));
localStorage.removeItem('key');
```

### Validate
```javascript
if (!value) return showError('Required');
if (!email.includes('@')) return showError('Invalid email');
if (password.length < 6) return showError('Too short');
```

---

## Master These First

1. **Fetch API** - GET, POST, PUT, DELETE
2. **localStorage** - setItem, getItem, removeItem
3. **Event Listeners** - click, submit, input, change
4. **DOM Manipulation** - innerHTML, textContent, classList
5. **Validation** - Check inputs before API call
6. **Error Handling** - .catch() or try/catch
7. **Promises** - .then() chaining
8. **JSON** - stringify(), parse()

Master these 8 topics → **Solve 90% of interview questions!** ✅