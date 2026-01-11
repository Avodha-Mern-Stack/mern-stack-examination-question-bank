# 5-Mark Questions on React (50 Questions)

## **JSX & React Fundamentals**

### 1. What is JSX in React? Explain its advantages with an example.
**Answer:**
JSX (JavaScript XML) is a syntax extension for JavaScript that allows writing HTML-like code within JavaScript. It's not HTML but gets transpiled to `React.createElement()` calls.

**Advantages:**
1. **Readability:** JSX is more intuitive and readable than plain JavaScript
2. **Type Safety:** Helps catch errors during compilation
3. **Developer Experience:** Provides syntax highlighting and code completion
4. **Performance:** Gets optimized during compilation
5. **JavaScript Power:** Can embed JavaScript expressions within curly braces

**Example:**
```jsx
// JSX
const element = <h1 className="title">Hello, {name}!</h1>;

// Transpiles to:
React.createElement('h1', {className: 'title'}, 'Hello, ', name, '!');

// With embedded JS
const isLoggedIn = true;
const greeting = (
  <div>
    {isLoggedIn ? (
      <h1>Welcome back!</h1>
    ) : (
      <h1>Please sign in.</h1>
    )}
  </div>
);
```

---

### 2. Explain the React component lifecycle. How is it handled in functional components?
**Answer:**
**Class Component Lifecycle:**
1. **Mounting:**
   - `constructor()`: Initializes state and binds methods
   - `static getDerivedStateFromProps()`: Updates state based on props
   - `render()`: Returns JSX
   - `componentDidMount()`: Executes after component mounts

2. **Updating:**
   - `static getDerivedStateFromProps()`: Before render
   - `shouldComponentUpdate()`: Determines if re-render is needed
   - `render()`: Re-renders component
   - `getSnapshotBeforeUpdate()`: Captures DOM info before update
   - `componentDidUpdate()`: Executes after update

3. **Unmounting:**
   - `componentWillUnmount()`: Cleanup before component removal

**Functional Components with Hooks:**
```jsx
import { useState, useEffect } from 'react';

function Example() {
  // Mounting/State initialization
  const [count, setCount] = useState(0);
  
  // Equivalent to componentDidMount + componentDidUpdate
  useEffect(() => {
    console.log('Component mounted or updated');
    
    // Cleanup function (equivalent to componentWillUnmount)
    return () => {
      console.log('Component will unmount');
    };
  }, [count]); // Dependency array controls when effect runs
  
  // Mounting only
  useEffect(() => {
    console.log('Component mounted only once');
  }, []);
  
  return <div>Count: {count}</div>;
}
```

---

### 3. What is prop drilling in React? How can it be avoided?
**Answer:**
**Prop Drilling:** Passing props through multiple intermediate components that don't need them, just to get them to deeply nested components that do need them.

**Example of Prop Drilling:**
```jsx
// GrandParent.js
function GrandParent() {
  const [user, setUser] = useState({ name: 'John' });
  return <Parent user={user} />;
}

// Parent.js (doesn't need user)
function Parent({ user }) {
  return <Child user={user} />;
}

// Child.js (doesn't need user)
function Child({ user }) {
  return <GrandChild user={user} />;
}

// GrandChild.js (needs user)
function GrandChild({ user }) {
  return <div>Hello, {user.name}</div>;
}
```

**Solutions to Avoid Prop Drilling:**

1. **Context API:**
```jsx
// UserContext.js
const UserContext = React.createContext();

// App.js
function App() {
  const [user, setUser] = useState({ name: 'John' });
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <GrandParent />
    </UserContext.Provider>
  );
}

// GrandChild.js
import { useContext } from 'react';
function GrandChild() {
  const { user } = useContext(UserContext);
  return <div>Hello, {user.name}</div>;
}
```

2. **Component Composition:**
```jsx
// Pass children/components directly
function Parent({ children }) {
  return <div>{children}</div>;
}

// Usage
<Parent>
  <Child user={user} />
</Parent>
```

3. **Render Props Pattern:**
```jsx
function UserProvider({ children }) {
  const [user, setUser] = useState({ name: 'John' });
  return children({ user, setUser });
}

// Usage
<UserProvider>
  {({ user }) => <GrandChild user={user} />}
</UserProvider>
```

4. **State Management Libraries** (Redux, MobX, Zustand)
5. **Higher-Order Components**

---

### 4. What are controlled and uncontrolled components in React? Differentiate with examples.
**Answer:**
**Controlled Components:** Form elements whose value is controlled by React state.

**Example:**
```jsx
function ControlledForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({ name, email });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Uncontrolled Components:** Form elements that maintain their own internal state.

**Example:**
```jsx
function UncontrolledForm() {
  const nameRef = useRef();
  const emailRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({
      name: nameRef.current.value,
      email: emailRef.current.value
    });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        ref={nameRef}
        placeholder="Name"
        defaultValue="John"
      />
      <input
        type="email"
        ref={emailRef}
        placeholder="Email"
        defaultValue="john@example.com"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

**Differences:**

| Aspect | Controlled Components | Uncontrolled Components |
|--------|---------------------|------------------------|
| **Value Source** | React state | DOM element |
| **Updates** | Through `onChange` | Through refs |
| **Validation** | Real-time validation | On submit/button click |
| **Form Reset** | Easy (set state to '') | Need DOM manipulation |
| **Performance** | Slightly slower (re-renders) | Better for simple forms |
| **Use Cases** | Complex forms, validation | Simple forms, file inputs |

**When to Use:**
- **Controlled:** When you need real-time validation, form state synchronization, or complex form logic
- **Uncontrolled:** When integrating with non-React code, for file inputs, or when performance is critical

---

### 5. Explain useState and useEffect hooks in detail with examples.
**Answer:**
**useState Hook:**
Manages state in functional components.

```jsx
import { useState } from 'react';

function Counter() {
  // Basic usage
  const [count, setCount] = useState(0);
  
  // Complex state (object)
  const [user, setUser] = useState({
    name: 'John',
    age: 25,
    email: 'john@example.com'
  });
  
  // Functional updates
  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };
  
  // Partial state updates
  const updateName = (newName) => {
    setUser(prevUser => ({
      ...prevUser,
      name: newName
    }));
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <p>Name: {user.name}</p>
      <button onClick={() => updateName('Jane')}>Change Name</button>
    </div>
  );
}
```

**useEffect Hook:**
Handles side effects in functional components.

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  // Effect with cleanup (componentDidMount + componentWillUnmount)
  useEffect(() => {
    console.log('Component mounted');
    
    // Cleanup function
    return () => {
      console.log('Component will unmount');
    };
  }, []);
  
  // Effect with dependencies (componentDidUpdate)
  useEffect(() => {
    const fetchUser = async () => {
      setLoading(true);
      try {
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        setUser(data);
      } catch (error) {
        console.error('Error fetching user:', error);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUser();
  }, [userId]); // Re-run when userId changes
  
  // Effect without dependencies (runs on every render)
  useEffect(() => {
    console.log('Rendered!');
  }); // No dependency array
  
  if (loading) return <div>Loading...</div>;
  return <div>User: {user?.name}</div>;
}
```

---

### 6. What is the Context API in React? When and how should it be used?
**Answer:**
**Context API** provides a way to pass data through the component tree without having to pass props down manually at every level.

**When to Use Context:**
1. **Theme** (light/dark mode)
2. **Authentication** (user info, login status)
3. **Language/Localization**
4. **Global preferences/settings**
5. **Avoiding prop drilling**

**How to Use:**
```jsx
// 1. Create Context
import React, { createContext, useState, useContext } from 'react';

const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {}
});

// 2. Create Provider Component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Create Custom Hook for easier consumption
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// 4. Using in Components
function App() {
  return (
    <ThemeProvider>
      <Header />
      <MainContent />
      <Footer />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header className={`header-${theme}`}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </header>
  );
}

function MainContent() {
  const { theme } = useTheme();
  
  return (
    <main className={`content-${theme}`}>
      <p>Current theme: {theme}</p>
    </main>
  );
}
```

**Best Practices:**
1. Split contexts for different concerns
2. Use custom hooks to consume context
3. Consider performance with frequent updates
4. Combine with `useMemo` for complex values

---

### 7. Explain React's Virtual DOM and reconciliation process.
**Answer:**
**Virtual DOM:** A lightweight JavaScript representation of the actual DOM.

**How it Works:**
1. **Initial Render:** React creates a virtual DOM tree
2. **State Change:** When state changes, React creates a new virtual DOM tree
3. **Diffing:** React compares new virtual DOM with previous one (reconciliation)
4. **Efficient Update:** Only updates changed parts in actual DOM

**Reconciliation Process:**
```javascript
// Example of reconciliation
const oldVDOM = {
  type: 'div',
  props: { className: 'container' },
  children: [
    { type: 'h1', props: {}, children: ['Hello'] },
    { type: 'p', props: {}, children: ['World'] }
  ]
};

// After state change
const newVDOM = {
  type: 'div',
  props: { className: 'container updated' },
  children: [
    { type: 'h1', props: {}, children: ['Hello'] },
    { type: 'p', props: {}, children: ['Updated World'] }
  ]
};

// React will only update:
// 1. className from 'container' to 'container updated'
// 2. Text content of <p> from 'World' to 'Updated World'
```

**Key Diffing Strategies:**
1. **Same Type Elements:** Updates attributes only, keeps DOM node
2. **Different Type Elements:** Replaces entire tree
3. **Keys:** Help identify elements in lists for efficient updates

**Example with Keys:**
```jsx
// Without keys (inefficient)
const items = ['Apple', 'Banana', 'Cherry'];
// React re-renders all items when array changes

// With keys (efficient)
const items = [
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Cherry' }
];
// React can identify which items changed, added, or removed
```

**Performance Benefits:**
1. **Minimal DOM Manipulation:** Only updates what changed
2. **Batched Updates:** Groups multiple state updates
3. **Efficient Diffing:** O(n) complexity with heuristic algorithm

---

### 8. What are Higher-Order Components (HOCs) in React? Explain with an example.
**Answer:**
**HOCs** are functions that take a component and return a new component with additional props or functionality.

**Characteristics:**
1. **Pure Function:** No side effects
2. **Component Composition:** Doesn't modify original component
3. **Props Proxy:** Passes through props to wrapped component

**Example 1: Basic HOC**
```jsx
// HOC for adding logging functionality
function withLogger(WrappedComponent) {
  return function LoggedComponent(props) {
    console.log(`Rendering ${WrappedComponent.name} with props:`, props);
    return <WrappedComponent {...props} />;
  };
}

// Usage
function MyComponent({ message }) {
  return <div>{message}</div>;
}

const EnhancedComponent = withLogger(MyComponent);

// Usage in JSX
<EnhancedComponent message="Hello World" />
```

**Example 2: HOC with Additional Props**
```jsx
// HOC for authentication
function withAuth(WrappedComponent) {
  return function AuthComponent(props) {
    const [isAuthenticated, setIsAuthenticated] = useState(false);
    const [user, setUser] = useState(null);
    
    const login = (credentials) => {
      // Authentication logic
      setIsAuthenticated(true);
      setUser({ name: 'John Doe' });
    };
    
    const logout = () => {
      setIsAuthenticated(false);
      setUser(null);
    };
    
    return (
      <WrappedComponent
        {...props}
        isAuthenticated={isAuthenticated}
        user={user}
        login={login}
        logout={logout}
      />
    );
  };
}

// Usage
function ProfilePage({ isAuthenticated, user, login, logout }) {
  if (!isAuthenticated) {
    return (
      <div>
        <h2>Please Login</h2>
        <button onClick={() => login({})}>Login</button>
      </div>
    );
  }
  
  return (
    <div>
      <h2>Welcome, {user?.name}</h2>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

const AuthenticatedProfile = withAuth(ProfilePage);
```

**Example 3: HOC for Data Fetching**
```jsx
function withDataFetching(url) {
  return function(WrappedComponent) {
    return function DataFetchingComponent(props) {
      const [data, setData] = useState(null);
      const [loading, setLoading] = useState(true);
      const [error, setError] = useState(null);
      
      useEffect(() => {
        const fetchData = async () => {
          try {
            const response = await fetch(url);
            const result = await response.json();
            setData(result);
          } catch (err) {
            setError(err.message);
          } finally {
            setLoading(false);
          }
        };
        
        fetchData();
      }, [url]);
      
      if (loading) return <div>Loading...</div>;
      if (error) return <div>Error: {error}</div>;
      
      return <WrappedComponent {...props} data={data} />;
    };
  };
}

// Usage
function UserList({ data }) {
  return (
    <ul>
      {data?.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

const UserListWithData = withDataFetching('/api/users')(UserList);
```

**HOC vs Hooks Comparison:**
```jsx
// HOC Pattern
const EnhancedComponent = withFeature(BaseComponent);

// Custom Hook Pattern
function useFeature() {
  // feature logic
}

function Component() {
  const feature = useFeature();
  // use feature
}
```

**Best Practices for HOCs:**
1. **Pass Unrelated Props:** Use spread operator to pass all props
2. **Display Name:** Set display name for debugging
3. **Don't Mutate:** Don't modify the wrapped component
4. **Composition:** Compose multiple HOCs when needed
5. **Forward Refs:** Use `React.forwardRef` if refs need to be passed

---

### 9. What is Redux? Explain its core concepts with examples.
**Answer:**
**Redux** is a predictable state container for JavaScript applications.

**Core Concepts:**

1. **Store:** Single source of truth for application state
2. **Actions:** Plain objects describing what happened
3. **Reducers:** Pure functions that specify how state changes
4. **Dispatch:** Method to send actions to the store

**Example:**
```javascript
// 1. Action Types
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';
const ADD_TODO = 'ADD_TODO';

// 2. Action Creators
const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { text, completed: false }
});

// 3. Reducer
const initialState = {
  count: 0,
  todos: []
};

function rootReducer(state = initialState, action) {
  switch (action.type) {
    case INCREMENT:
      return { ...state, count: state.count + 1 };
    case DECREMENT:
      return { ...state, count: state.count - 1 };
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
    default:
      return state;
  }
}

// 4. Create Store (Redux)
import { createStore } from 'redux';
const store = createStore(rootReducer);

// 5. Subscribe to changes
store.subscribe(() => {
  console.log('State updated:', store.getState());
});

// 6. Dispatch actions
store.dispatch(increment()); // count: 1
store.dispatch(increment()); // count: 2
store.dispatch(decrement()); // count: 1
store.dispatch(addTodo('Learn Redux'));
```

**Redux Toolkit (Modern Approach):**
```javascript
import { createSlice, configureStore } from '@reduxjs/toolkit';

// Create slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => {
      state.value += 1;
    },
    decrement: state => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

// Export actions and reducer
export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;

// Create store
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer
  }
});

// In React Component
import { useSelector, useDispatch } from 'react-redux';

function Counter() {
  const count = useSelector(state => state.counter.value);
  const dispatch = useDispatch();
  
  return (
    <div>
      <span>{count}</span>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}
```

---

### 10. Explain React Router and its key components with examples.
**Answer:**
**React Router** is a library for routing in React applications.

**Key Components:**

1. **BrowserRouter:** Wrapper for the application
2. **Routes:** Container for Route components (v6)
3. **Route:** Defines path and component to render
4. **Link:** Navigation without page reload
5. **Navigate:** Programmatic navigation/redirects
6. **Outlet:** Renders child routes

**Example:**
```jsx
import {
  BrowserRouter,
  Routes,
  Route,
  Link,
  Navigate,
  Outlet,
  useParams,
  useNavigate
} from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/users">Users</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        
        {/* Nested Routes */}
        <Route path="/users" element={<UsersLayout />}>
          <Route index element={<UserList />} />
          <Route path=":userId" element={<UserProfile />} />
          <Route path="new" element={<NewUser />} />
        </Route>
        
        {/* Redirect */}
        <Route path="/old-route" element={<Navigate to="/new-route" />} />
        
        {/* 404 Route */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

// Layout component with nested routes
function UsersLayout() {
  return (
    <div>
      <h2>Users Section</h2>
      <Outlet /> {/* Renders child routes here */}
    </div>
  );
}

// Component with route parameters
function UserProfile() {
  const { userId } = useParams();
  const navigate = useNavigate();
  
  return (
    <div>
      <h3>User ID: {userId}</h3>
      <button onClick={() => navigate('/users')}>
        Back to Users
      </button>
    </div>
  );
}

// Protected Route Example
function PrivateRoute({ children }) {
  const isAuthenticated = true; // Check auth status
  
  return isAuthenticated ? children : <Navigate to="/login" />;
}

// Usage
<Route
  path="/dashboard"
  element={
    <PrivateRoute>
      <Dashboard />
    </PrivateRoute>
  }
/>
```

**Additional Features:**

1. **Lazy Loading:**
```jsx
const About = React.lazy(() => import('./About'));
const Contact = React.lazy(() => import('./Contact'));

<Routes>
  <Route path="/about" element={
    <Suspense fallback={<div>Loading...</div>}>
      <About />
    </Suspense>
  } />
</Routes>
```

2. **Query Parameters:**
```jsx
import { useLocation } from 'react-router-dom';

function SearchResults() {
  const location = useLocation();
  const queryParams = new URLSearchParams(location.search);
  const searchTerm = queryParams.get('q');
  
  return <div>Searching for: {searchTerm}</div>;
}

// Navigate with query params
<Link to={`/search?q=${searchTerm}&page=1`}>Search</Link>
```

3. **Programmatic Navigation:**
```jsx
function LoginForm() {
  const navigate = useNavigate();
  
  const handleLogin = async () => {
    // Login logic
    navigate('/dashboard', { 
      replace: true,
      state: { from: location.pathname }
    });
  };
  
  return <button onClick={handleLogin}>Login</button>;
}
```

---

### 11. What are React Portals? When and how to use them?
**Answer:**
**React Portals** provide a way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.

**When to Use Portals:**
1. **Modals/Dialogs:** To avoid z-index and overflow issues
2. **Tooltips:** Render outside container bounds
3. **Dropdowns:** Escape overflow:hidden containers
4. **Notifications/Toasts:** Render at root level
5. **Context Menus:** Position anywhere in viewport

**How to Use:**
```jsx
import ReactDOM from 'react-dom';

// 1. Modal Component using Portal
function Modal({ children, onClose }) {
  return ReactDOM.createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.getElementById('modal-root') // Portal target
  );
}

// 2. App Component
function App() {
  const [isModalOpen, setIsModalOpen] = useState(false);
  
  return (
    <div className="app">
      <h1>My App</h1>
      <button onClick={() => setIsModalOpen(true)}>
        Open Modal
      </button>
      
      {isModalOpen && (
        <Modal onClose={() => setIsModalOpen(false)}>
          <h2>Modal Title</h2>
          <p>This is rendered outside the app DOM hierarchy.</p>
        </Modal>
      )}
    </div>
  );
}

// 3. HTML (index.html)
<div id="root"></div>
<div id="modal-root"></div> {/* Portal target */}
```

**Example 2: Tooltip with Portal**
```jsx
function Tooltip({ children, content, position = 'top' }) {
  const [isVisible, setIsVisible] = useState(false);
  const [coords, setCoords] = useState({ x: 0, y: 0 });
  const ref = useRef();
  
  const showTooltip = () => {
    const rect = ref.current.getBoundingClientRect();
    setCoords({
      x: rect.left + rect.width / 2,
      y: rect.top
    });
    setIsVisible(true);
  };
  
  const tooltipStyle = {
    position: 'fixed',
    left: coords.x,
    top: position === 'top' ? coords.y - 40 : coords.y + 40,
    transform: 'translateX(-50%)',
    zIndex: 1000
  };
  
  return (
    <>
      <span
        ref={ref}
        onMouseEnter={showTooltip}
        onMouseLeave={() => setIsVisible(false)}
      >
        {children}
      </span>
      
      {isVisible && ReactDOM.createPortal(
        <div className="tooltip" style={tooltipStyle}>
          {content}
        </div>,
        document.body
      )}
    </>
  );
}

// Usage
<Tooltip content="This is a tooltip">
  <button>Hover me</button>
</Tooltip>
```

**Benefits of Portals:**
1. **Escape DOM Hierarchy:** Render outside parent containers
2. **Event Bubbling:** Events still bubble through React tree
3. **CSS Isolation:** Avoid parent container CSS affecting modal
4. **Z-index Management:** Easy to manage stacking context

---

### 12. Explain React.memo(), useMemo(), and useCallback() hooks.
**Answer:**
These are performance optimization hooks that help prevent unnecessary re-renders.

**1. React.memo():**
Memoizes a component to prevent re-renders when props haven't changed.

```jsx
// Without memo - re-renders on parent re-render
function ExpensiveComponent({ data }) {
  console.log('ExpensiveComponent rendered');
  // Expensive computation here
  return <div>{data}</div>;
}

// With memo - only re-renders when props change
const MemoizedComponent = React.memo(ExpensiveComponent);

// Custom comparison function
const MemoizedWithCustomCompare = React.memo(
  ExpensiveComponent,
  (prevProps, nextProps) => {
    // Return true if props are equal (skip re-render)
    return prevProps.data.id === nextProps.data.id;
  }
);

// Usage
function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <button onClick={() => setCount(c => c + 1)}>
        Count: {count}
      </button>
      <MemoizedComponent data="static data" />
    </div>
  );
}
```

**2. useMemo():**
Memoizes a computed value to avoid expensive calculations on every render.

```jsx
function ExpensiveCalculation({ items, filter }) {
  // This runs on every render without useMemo
  const filteredItems = useMemo(() => {
    console.log('Filtering items...');
    return items.filter(item => 
      item.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [items, filter]); // Only recalculates when dependencies change
  
  // Another example: sorting
  const sortedItems = useMemo(() => {
    return [...filteredItems].sort((a, b) => 
      a.name.localeCompare(b.name)
    );
  }, [filteredItems]);
  
  return (
    <ul>
      {sortedItems.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}

// Example with complex object
function UserProfile({ userId }) {
  const userConfig = useMemo(() => ({
    userId,
    theme: 'dark',
    permissions: ['read', 'write'],
    // Expensive initialization
    ...computeExpensiveConfig(userId)
  }), [userId]);
  
  return <UserSettings config={userConfig} />;
}
```

**3. useCallback():**
Memoizes a function to prevent creating new function instances on every render.

```jsx
function Parent() {
  const [count, setCount] = useState(0);
  const [items, setItems] = useState([]);
  
  // Without useCallback - new function on every render
  const handleAddItem = () => {
    setItems([...items, `Item ${items.length}`]);
  };
  
  // With useCallback - same function unless dependencies change
  const memoizedHandleAddItem = useCallback(() => {
    setItems(prevItems => [...prevItems, `Item ${prevItems.length}`]);
  }, []); // Empty array = never changes
  
  // Example with parameters
  const handleRemoveItem = useCallback((index) => {
    setItems(prevItems => prevItems.filter((_, i) => i !== index));
  }, []);
  
  // Example with dependency
  const handleUpdateItem = useCallback((index, newValue) => {
    setItems(prevItems => 
      prevItems.map((item, i) => i === index ? newValue : item)
    );
  }, []); // No dependencies needed with functional updates
  
  return (
    <div>
      <ChildComponent onAdd={memoizedHandleAddItem} />
      <List items={items} onRemove={handleRemoveItem} />
    </div>
  );
}

function ChildComponent({ onAdd }) {
  // Without useCallback, this would re-render on every parent render
  // because onAdd prop would be a new function each time
  return <button onClick={onAdd}>Add Item</button>;
}

// Optimized with React.memo
const MemoizedChild = React.memo(ChildComponent);
```

**When to Use Each:**

| Hook | Purpose | When to Use |
|------|---------|-------------|
| **React.memo()** | Memoize component | Pure components, expensive renders |
| **useMemo()** | Memoize value | Expensive computations, object creation |
| **useCallback()** | Memoize function | Passing callbacks to memoized children |

**Performance Example:**
```jsx
function OptimizedApp() {
  const [search, setSearch] = useState('');
  const [users, setUsers] = useState([]);
  
  // Memoized filter function
  const filteredUsers = useMemo(() => {
    return users.filter(user => 
      user.name.toLowerCase().includes(search.toLowerCase())
    );
  }, [users, search]);
  
  // Memoized event handler
  const handleSearch = useCallback((value) => {
    setSearch(value);
  }, []);
  
  // Memoized add user handler
  const handleAddUser = useCallback((name) => {
    setUsers(prev => [...prev, { id: Date.now(), name }]);
  }, []);
  
  return (
    <div>
      <SearchInput onSearch={handleSearch} />
      <UserList users={filteredUsers} />
      <AddUserButton onAdd={handleAddUser} />
    </div>
  );
}

// Child components
const SearchInput = React.memo(({ onSearch }) => (
  <input onChange={e => onSearch(e.target.value)} />
));

const UserList = React.memo(({ users }) => (
  <ul>
    {users.map(user => (
      <li key={user.id}>{user.name}</li>
    ))}
  </ul>
));

const AddUserButton = React.memo(({ onAdd }) => (
  <button onClick={() => onAdd('New User')}>Add User</button>
));
```

---

### 13. What is Server-Side Rendering (SSR) in React? Explain Next.js.
**Answer:**
**Server-Side Rendering (SSR)** renders React components on the server and sends HTML to the client.

**Traditional CSR vs SSR:**
- **CSR (Client-Side):** Browser downloads empty HTML, then loads JS and renders
- **SSR:** Server renders HTML, sends complete page to browser

**Benefits of SSR:**
1. **SEO:** Search engines can crawl content
2. **Performance:** Faster initial page load
3. **Social Media:** Proper meta tags for sharing
4. **Accessibility:** Content available without JavaScript

**Next.js** is a React framework that provides SSR, SSG, and other features out of the box.

**Next.js Features:**

1. **Page-based Routing:**
```jsx
// pages/index.js
export default function Home() {
  return <h1>Home Page</h1>;
}

// pages/about.js
export default function About() {
  return <h1>About Page</h1>;
}

// Dynamic routes
// pages/users/[id].js
import { useRouter } from 'next/router';

export default function User() {
  const router = useRouter();
  const { id } = router.query;
  
  return <h1>User ID: {id}</h1>;
}
```

2. **Data Fetching Methods:**

**getServerSideProps (SSR):**
```jsx
export async function getServerSideProps(context) {
  // Runs on every request
  const res = await fetch(`https://api.example.com/data`);
  const data = await res.json();
  
  return {
    props: { data } // Passed to component as props
  };
}

function Page({ data }) {
  return <div>{JSON.stringify(data)}</div>;
}
```

**getStaticProps (SSG):**
```jsx
export async function getStaticProps() {
  // Runs at build time
  const res = await fetch(`https://api.example.com/posts`);
  const posts = await res.json();
  
  return {
    props: { posts },
    revalidate: 10 // Regenerate every 10 seconds (ISR)
  };
}
```

**getStaticPaths (Dynamic SSG):**
```jsx
export async function getStaticPaths() {
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();
  
  const paths = posts.map(post => ({
    params: { id: post.id.toString() }
  }));
  
  return { paths, fallback: true };
}

export async function getStaticProps({ params }) {
  const res = await fetch(`https://api.example.com/posts/${params.id}`);
  const post = await res.json();
  
  return { props: { post } };
}
```

3. **API Routes:**
```jsx
// pages/api/users.js
export default function handler(req, res) {
  if (req.method === 'GET') {
    res.status(200).json([{ id: 1, name: 'John' }]);
  } else if (req.method === 'POST') {
    // Handle POST request
    res.status(201).json({ message: 'User created' });
  }
}
```

4. **Image Optimization:**
```jsx
import Image from 'next/image';

function MyComponent() {
  return (
    <Image
      src="/profile.jpg"
      alt="Profile"
      width={500}
      height={500}
      priority // Load with priority
    />
  );
}
```

5. **Styling Support:**
```jsx
// CSS Modules
import styles from './Button.module.css';

function Button() {
  return <button className={styles.primary}>Click</button>;
}

// Global CSS
import '../styles/globals.css';

// CSS-in-JS
import styled from 'styled-components';

const Title = styled.h1`
  color: ${props => props.color || 'black'};
`;
```

6. **Environment Variables:**
```javascript
// .env.local
API_KEY=your_api_key

// Access in code
const apiKey = process.env.API_KEY;
```

**Complete Next.js App Example:**
```jsx
// pages/index.js
import Head from 'next/head';
import Link from 'next/link';

export default function Home({ posts }) {
  return (
    <>
      <Head>
        <title>My Blog</title>
        <meta name="description" content="A simple blog" />
      </Head>
      
      <h1>Welcome to my blog</h1>
      <ul>
        {posts.map(post => (
          <li key={post.id}>
            <Link href={`/posts/${post.id}`}>
              {post.title}
            </Link>
          </li>
        ))}
      </ul>
    </>
  );
}

export async function getStaticProps() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  const posts = await res.json();
  
  return {
    props: {
      posts: posts.slice(0, 10)
    }
  };
}

// pages/posts/[id].js
export default function Post({ post }) {
  if (!post) return <div>Loading...</div>;
  
  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </article>
  );
}

export async function getStaticPaths() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  const posts = await res.json();
  
  const paths = posts.slice(0, 10).map(post => ({
    params: { id: post.id.toString() }
  }));
  
  return { paths, fallback: true };
}

export async function getStaticProps({ params }) {
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/posts/${params.id}`
  );
  const post = await res.json();
  
  return { props: { post } };
}
```

---

### 14. Explain Error Boundaries in React with examples.
**Answer:**
**Error Boundaries** are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of crashing.

**Characteristics:**
1. Only catch errors in children, not in themselves
2. Only catch errors during rendering, lifecycle methods, and constructors
3. Don't catch errors in event handlers, async code, or SSR

**Creating an Error Boundary:**
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { 
      hasError: false,
      error: null,
      errorInfo: null
    };
  }
  
  static getDerivedStateFromError(error) {
    // Update state so the next render shows fallback UI
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    // Log error to error reporting service
    console.error('Error caught by boundary:', error, errorInfo);
    this.setState({
      error: error,
      errorInfo: errorInfo
    });
  }
  
  render() {
    if (this.state.hasError) {
      // Fallback UI
      return this.props.fallback || (
        <div className="error-boundary">
          <h2>Something went wrong.</h2>
          <details style={{ whiteSpace: 'pre-wrap' }}>
            {this.state.error && this.state.error.toString()}
            <br />
            {this.state.errorInfo && this.state.errorInfo.componentStack}
          </details>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

**Usage Examples:**

**Example 1: Basic Usage**
```jsx
function App() {
  return (
    <ErrorBoundary fallback={<h2>App Error!</h2>}>
      <BuggyComponent />
    </ErrorBoundary>
  );
}

function BuggyComponent() {
  const [items, setItems] = useState([]);
  
  // This will crash if items is undefined
  return <div>{items.map(item => item.name)}</div>;
}
```

**Example 2: Granular Error Boundaries**
```jsx
function App() {
  return (
    <div className="app">
      <Header />
      
      <ErrorBoundary fallback={<SidebarError />}>
        <Sidebar />
      </ErrorBoundary>
      
      <main>
        <ErrorBoundary fallback={<MainContentError />}>
          <MainContent />
        </ErrorBoundary>
      </main>
      
      <ErrorBoundary fallback={null}> {/* Silently fail */}
        <Widget />
      </ErrorBoundary>
    </div>
  );
}

function SidebarError() {
  return (
    <div className="sidebar-error">
      <p>Sidebar is temporarily unavailable.</p>
    </div>
  );
}
```

**Example 3: Error Boundary with Recovery**
```jsx
class RecoverableErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { 
      hasError: false,
      errorCount: 0,
      lastError: null
    };
  }
  
  static getDerivedStateFromError(error) {
    return { 
      hasError: true,
      lastError: error 
    };
  }
  
  componentDidCatch(error, errorInfo) {
    this.setState(prev => ({
      errorCount: prev.errorCount + 1
    }));
    
    // Report to monitoring service
    reportErrorToService(error, errorInfo);
  }
  
  handleRetry = () => {
    this.setState({ hasError: false });
  };
  
  handleReset = () => {
    this.setState({ 
      hasError: false,
      errorCount: 0,
      lastError: null 
    });
  };
  
  render() {
    if (this.state.hasError) {
      return (
        <div className="error-recovery">
          <h3>Oops! Something went wrong.</h3>
          <p>Error count: {this.state.errorCount}</p>
          
          {this.state.errorCount < 3 ? (
            <button onClick={this.handleRetry}>
              Try Again
            </button>
          ) : (
            <div>
              <p>Multiple errors occurred.</p>
              <button onClick={this.handleReset}>
                Reset Component
              </button>
            </div>
          )}
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

**Example 4: Functional Component with Error Boundary**
```jsx
// Custom hook for error boundaries in functional components
function useErrorBoundary() {
  const [error, setError] = useState(null);
  
  const ErrorBoundary = useCallback(({ children }) => {
    if (error) {
      return (
        <div>
          <h2>Error: {error.message}</h2>
          <button onClick={() => setError(null)}>Dismiss</button>
        </div>
      );
    }
    
    try {
      return children;
    } catch (err) {
      setError(err);
      return null;
    }
  }, [error]);
  
  return { ErrorBoundary, setError };
}

// Usage
function App() {
  const { ErrorBoundary } = useErrorBoundary();
  
  return (
    <ErrorBoundary>
      <BuggyComponent />
    </ErrorBoundary>
  );
}
```

**Example 5: Error Boundary for Async Operations**
```jsx
class AsyncErrorBoundary extends React.Component {
  state = { hasError: false, promiseError: null };
  
  componentDidMount() {
    // Handle unhandled promise rejections
    window.addEventListener('unhandledrejection', this.handlePromiseRejection);
  }
  
  componentWillUnmount() {
    window.removeEventListener('unhandledrejection', this.handlePromiseRejection);
  }
  
  handlePromiseRejection = (event) => {
    event.preventDefault();
    this.setState({
      hasError: true,
      promiseError: event.reason
    });
  };
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Async error:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h2>Async operation failed</h2>
          <p>{this.state.promiseError?.message}</p>
        </div>
      );
    }
    
    return this.props.children;
  }
}
```

**Best Practices:**
1. Place error boundaries at strategic locations
2. Use different fallbacks for different sections
3. Log errors to monitoring services
4. Provide recovery options when possible
5. Test error boundaries with simulated errors

---

### 15. What are React Fragments? Why and when to use them?
**Answer:**
**React Fragments** let you group multiple children without adding extra DOM nodes.

**Why Use Fragments:**
1. **Cleaner DOM:** Avoid unnecessary wrapper divs
2. **CSS/Styling:** No interference from wrapper elements
3. **Flexbox/Grid:** Wrapper divs can break layouts
4. **Semantic HTML:** Maintain proper HTML structure
5. **Performance:** Slightly better with fewer DOM nodes

**Syntax Options:**
```jsx
// 1. Explicit Fragment syntax
function List() {
  return (
    <React.Fragment>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </React.Fragment>
  );
}

// 2. Short syntax (most common)
function List() {
  return (
    <>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </>
  );
}

// 3. With keys (only available with explicit syntax)
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

**When to Use Fragments:**

**Example 1: Returning Multiple Elements**
```jsx
// Without Fragment (adds extra div)
function Component() {
  return (
    <div> {/* Unnecessary wrapper */}
      <Header />
      <MainContent />
      <Footer />
    </div>
  );
}

// With Fragment (cleaner output)
function Component() {
  return (
    <>
      <Header />
      <MainContent />
      <Footer />
    </>
  );
}
```

**Example 2: Table Structures**
```jsx
// Problem: Div breaks table structure
function Table() {
  return (
    <table>
      <tr>
        <Columns /> {/* This would return divs! */}
      </tr>
    </table>
  );
}

function Columns() {
  return (
    <div> {/* Breaks table! */}
      <td>Column 1</td>
      <td>Column 2</td>
    </div>
  );
}

// Solution: Use Fragment
function Columns() {
  return (
    <>
      <td>Column 1</td>
      <td>Column 2</td>
    </>
  );
}
```

**Example 3: CSS Grid/Flexbox Layouts**
```jsx
// Without Fragment - wrapper div breaks grid
function GridLayout() {
  return (
    <div className="grid-container">
      <div> {/* This extra div breaks the grid! */}
        <Card />
        <Card />
        <Card />
      </div>
    </div>
  );
}

// With Fragment - clean grid structure
function GridLayout() {
  return (
    <div className="grid-container">
      <>
        <Card />
        <Card />
        <Card />
      </>
    </div>
  );
}
```

**Example 4: Conditional Rendering**
```jsx
function UserProfile({ user }) {
  return (
    <>
      {user && (
        <>
          <h1>{user.name}</h1>
          <p>{user.email}</p>
          <img src={user.avatar} alt={user.name} />
        </>
      )}
      {!user && <p>Loading user data...</p>}
    </>
  );
}
```

**Example 5: Mapping with Keys**
```jsx
function BlogPost({ post }) {
  return (
    <article>
      <h2>{post.title}</h2>
      {post.sections.map(section => (
        <React.Fragment key={section.id}>
          <h3>{section.heading}</h3>
          <p>{section.content}</p>
          {section.code && <pre>{section.code}</pre>}
        </React.Fragment>
      ))}
    </article>
  );
}
```

**When NOT to Use Fragments:**
1. **When you need refs:** Fragments can't have refs
2. **When you need event handlers:** Attach to actual elements instead
3. **When wrapper provides necessary styling/functionality**

**Comparison with Other Solutions:**
```jsx
// Option 1: Array (requires keys)
return [
  <div key="1">Item 1</div>,
  <div key="2">Item 2</div>
];

// Option 2: Fragment (cleaner)
return (
  <>
    <div>Item 1</div>
    <div>Item 2</div>
  </>
);
```

**Performance Consideration:**
Fragments have minimal performance impact but can help with:
1. Reduced DOM nodes
2. Better memory usage
3. Faster rendering in some cases

---
