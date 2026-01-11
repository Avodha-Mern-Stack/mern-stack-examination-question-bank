**PART B Questions**

---

### **1. Explain the difference between React state and Redux store.**

**Answer:** (5 marks)
- **React State**: Local component state managed with `useState()` or `setState()`. It's component-specific and not accessible to other components unless passed via props.
- **Redux Store**: Global application state managed in a central store. Any component can access/update it via `connect()` or hooks like `useSelector()`. It follows a unidirectional data flow with actions and reducers.
- **Key Differences**: 
  - Scope: Local vs Global
  - Data Flow: Props drilling vs Centralized store
  - Use Case: Component-specific UI state vs App-wide shared state
  - Performance: Optimized re-renders vs Predictable state updates
  - Boilerplate: Minimal vs More setup required

---

### **2. Describe how to connect a MongoDB database with Express using Mongoose.**

**Answer:** (5 marks)
1. Install Mongoose: `npm install mongoose`
2. Require and connect:
```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/dbname', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
```
3. Define schema and model:
```javascript
const userSchema = new mongoose.Schema({
  name: String,
  email: String
});
const User = mongoose.model('User', userSchema);
```
4. Use in Express routes:
```javascript
app.post('/users', async (req, res) => {
  const user = new User(req.body);
  await user.save();
  res.send(user);
});
```
5. Handle connection events and errors for production readiness.

---

### **3. What is the difference between PUT and PATCH methods in REST APIs?**

**Answer:** (5 marks)
- **PUT**: Complete replacement of a resource. Client sends the entire updated resource; server replaces it completely. Idempotent.
- **PATCH**: Partial update of a resource. Client sends only changed fields; server updates only those fields. Not necessarily idempotent.
- **Example**: 
  - PUT `/users/1` → Replace entire user object
  - PATCH `/users/1` → Update only email field
- **When to use**: PUT when replacing entire resource, PATCH when updating specific fields.
- **Idempotency**: PUT is idempotent (same request yields same result), PATCH may not be.

---

### **4. Explain middleware in Express and give an example of its use.**

**Answer:** (5 marks)
Middleware are functions that have access to request/response objects and can:
1. Execute any code
2. Modify request/response
3. End request-response cycle
4. Call next middleware

**Example: Logging middleware**
```javascript
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date()}`);
  next(); // Pass control to next middleware
};
app.use(logger);
```

**Other examples**: Authentication, body parsing, error handling, CORS.

---

### **5. What is the purpose of CORS middleware in Express and how is it used?**

**Answer:** (5 marks)
- **Purpose**: Enables Cross-Origin Resource Sharing, allowing web applications on one domain to access resources from another domain (bypassing Same-Origin Policy).
- **Installation**: `npm install cors`
- **Usage**: 
```javascript
const cors = require('cors');
// Enable for all routes
app.use(cors());
// Or configure specific origins
app.use(cors({
  origin: 'http://localhost:3000',
  methods: ['GET', 'POST'],
  credentials: true
}));
```
- **Why needed**: Browser security policy blocks cross-origin requests by default.
- **Headers added**: `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, etc.

---

### **6. Explain React component lifecycle methods with examples.**

**Answer:** (5 marks)
1. **Mounting**: 
   - `constructor()` - Initial setup
   - `render()` - Returns JSX
   - `componentDidMount()` - API calls, subscriptions
2. **Updating**:
   - `shouldComponentUpdate()` - Performance optimization
   - `render()` - Re-render
   - `componentDidUpdate()` - DOM updates after render
3. **Unmounting**:
   - `componentWillUnmount()` - Cleanup
4. **Error Handling**:
   - `componentDidCatch()` - Error boundaries

---

### **7. Describe Redux data flow with diagram explanation.**

**Answer:** (5 marks)
1. **Action Created**: User interaction triggers action creator
2. **Action Dispatched**: `dispatch(action)` to store
3. **Reducer Processes**: Store passes action to reducer
4. **State Updated**: Reducer returns new state
5. **View Updates**: Components re-render with new state
```
View → Action → Dispatch → Reducer → Store → View
```
- **Pure Functions**: Reducers must be pure
- **Immutability**: State updates create new objects
- **Single Source**: All state in single store

---

### **8. What are React Hooks? Explain useState and useEffect with examples.**

**Answer:** (5 marks)
- **Hooks**: Functions that let you use React features in functional components.
- **useState**: 
```javascript
const [count, setCount] = useState(0);
setCount(count + 1);
```
- **useEffect**: 
```javascript
useEffect(() => {
  // Side effect
  document.title = `Count: ${count}`;
  return () => {
    // Cleanup
  };
}, [count]); // Dependency array
```
- **Rules**: Only call at top level, only from React functions.
- **Benefits**: Reusable stateful logic, no class components needed.

---

### **9. Explain MongoDB aggregation pipeline with example.**

**Answer:** (5 marks)
Aggregation pipeline processes documents through stages:
1. **$match**: Filter documents
2. **$group**: Group by field
3. **$sort**: Sort results
4. **$project**: Reshape documents

**Example**: Find average salary by department
```javascript
db.employees.aggregate([
  { $match: { active: true } },
  { $group: {
    _id: "$department",
    avgSalary: { $avg: "$salary" },
    count: { $sum: 1 }
  }},
  { $sort: { avgSalary: -1 } }
])
```

---

### **10. What is JWT authentication? Explain its implementation in MERN.**

**Answer:** (5 marks)
- **JWT**: JSON Web Token - stateless authentication token
- **Structure**: Header.Payload.Signature
- **Flow**: 
  1. User login → Server validates → Returns JWT
  2. Client stores token (localStorage/HTTP-only cookie)
  3. Subsequent requests include token in Authorization header
  4. Server verifies token signature → Grants access
- **Implementation**:
  - `jsonwebtoken` package
  - `jwt.sign()` to create, `jwt.verify()` to validate
  - Middleware to protect routes
  - Refresh token strategy for security

---

### **11. Explain React Router with implementation of protected routes.**

**Answer:** (5 marks)
```javascript
// ProtectedRoute component
const ProtectedRoute = ({ children }) => {
  const { user } = useAuth();
  return user ? children : <Navigate to="/login" />;
};

// Route setup
<Routes>
  <Route path="/login" element={<Login />} />
  <Route path="/dashboard" element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  } />
</Routes>
```
- **Features**: Nested routes, dynamic routing, navigation guards
- **Hooks**: `useNavigate()`, `useParams()`, `useLocation()`

---

### **12. What are MongoDB indexes? Explain types and when to use them.**

**Answer:** (5 marks)
- **Purpose**: Improve query performance (like book index)
- **Types**:
  1. **Single Field**: On one field
  2. **Compound**: On multiple fields
  3. **Multikey**: On array fields
  4. **Text**: For text search
  5. **Geospatial**: For location data
- **When to index**: 
  - Frequently queried fields
  - Fields used in sorts
  - Fields with high selectivity
- **Trade-off**: Faster reads, slower writes, storage overhead

---

### **13. Explain error handling in Express with proper HTTP status codes.**

**Answer:** (5 marks)
```javascript
// Custom error middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    error: {
      message: err.message,
      status: err.status
    }
  });
});

// Common status codes:
// 200 OK, 201 Created
// 400 Bad Request, 401 Unauthorized
// 403 Forbidden, 404 Not Found
// 500 Internal Server Error
```
- **Best Practices**: Centralized error handling, meaningful messages, logging.

---

### **14. What is code splitting in React? Explain with implementation.**

**Answer:** (5 marks)
- **Purpose**: Split code into smaller bundles, load on demand
- **React.lazy()**: 
```javascript
const LazyComponent = React.lazy(() => import('./HeavyComponent'));
```
- **Suspense**: 
```javascript
<Suspense fallback={<Spinner />}>
  <LazyComponent />
</Suspense>
```
- **Benefits**: Faster initial load, better performance
- **Route-based splitting**: Split by routes for optimal loading

---

### **15. Explain Mongoose schema validation and middleware.**

**Answer:** (5 marks)
```javascript
const schema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    match: /^\S+@\S+\.\S+$/,
    trim: true,
    lowercase: true
  },
  age: {
    type: Number,
    min: 18,
    max: 100
  }
});

// Middleware examples
schema.pre('save', function(next) {
  // Before saving
  next();
});

schema.post('save', function(doc) {
  // After saving
});
```
- **Types**: String, Number, Date, Buffer, Boolean, Mixed, ObjectId, Array
- **Validators**: required, min/max, match, enum, custom

---

### **16. What are controlled vs uncontrolled components in React?**

**Answer:** (5 marks)
- **Controlled**: Form data handled by React state
```javascript
<input value={name} onChange={e => setName(e.target.value)} />
```
- **Uncontrolled**: Form data handled by DOM
```javascript
const inputRef = useRef();
<input ref={inputRef} defaultValue="initial" />
```
- **When to use**: Controlled for validation, dynamic forms; Uncontrolled for simple forms, file inputs.
- **Benefits**: Controlled gives full control, validation; Uncontrolled less code, better performance.

---

### **17. Explain MongoDB transactions with implementation.**

**Answer:** (5 marks)
```javascript
const session = await mongoose.startSession();
try {
  session.startTransaction();
  
  await User.updateOne(
    { _id: userId },
    { $inc: { balance: -100 } },
    { session }
  );
  
  await Payment.create([{ userId, amount: 100 }], { session });
  
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
  throw error;
} finally {
  session.endSession();
}
```
- **ACID Properties**: Atomicity, Consistency, Isolation, Durability
- **Use Cases**: Financial transactions, inventory management
- **Requirements**: Replica set for MongoDB < 4.2

---

### **18. What are React Context and useContext hook? Explain with example.**

**Answer:** (5 marks)
```javascript
// Create context
const ThemeContext = createContext();

// Provider
<ThemeContext.Provider value="dark">
  <App />
</ThemeContext.Provider>

// Consumer with useContext
const theme = useContext(ThemeContext);

// Class alternative: ThemeContext.Consumer
```
- **Purpose**: Avoid prop drilling, share global data
- **When to use**: Theme, user auth, language preferences
- **Limitations**: Not for frequently changing data, performance considerations

---

### **19. Explain Express routing and route parameters.**

**Answer:** (5 marks)
```javascript
// Basic routing
app.get('/users', (req, res) => {});
app.post('/users', (req, res) => {});

// Route parameters
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
});

// Query parameters
// /users?page=1&limit=10
app.get('/users', (req, res) => {
  const { page, limit } = req.query;
});

// Route modularization
const userRouter = express.Router();
userRouter.get('/profile', (req, res) => {});
app.use('/users', userRouter);
```

---

### **20. What is virtualization in React? Explain with implementation.**

**Answer:** (5 marks)
- **Problem**: Rendering large lists causes performance issues
- **Solution**: Render only visible items (window/windowing)
- **Library**: `react-window` or `react-virtualized`
```javascript
import { FixedSizeList as List } from 'react-window';

const Row = ({ index, style }) => (
  <div style={style}>Row {index}</div>
);

<List height={400} itemCount={1000} itemSize={35}>
  {Row}
</List>
```
- **Benefits**: Better performance, memory efficiency
- **When to use**: Lists with 100+ items, large tables

---

### **21. Explain MongoDB replication and sharding.**

**Answer:** (5 marks)
- **Replication**: Multiple copies of data (primary + secondaries)
  - **Purpose**: High availability, disaster recovery
  - **Setup**: Replica set with 3+ nodes
  - **Failover**: Automatic primary election
- **Sharding**: Horizontal partitioning across machines
  - **Purpose**: Handle large datasets, high throughput
  - **Components**: Shards (data), mongos (router), config servers
  - **Shard Key**: Determines data distribution
- **Use Cases**: Replication for HA, Sharding for scalability

---

### **22. What are React Portals? Explain with use cases.**

**Answer:** (5 marks)
```javascript
// Create portal
const Modal = ({ children }) => {
  return ReactDOM.createPortal(
    children,
    document.getElementById('modal-root')
  );
};

// Usage
<Modal>
  <div>Modal Content</div>
</Modal>
```
- **Purpose**: Render children outside DOM hierarchy
- **Use Cases**: Modals, tooltips, dialogs, notifications
- **Benefits**: Escape CSS overflow:hidden, manage z-index properly
- **Event Bubbling**: Events bubble through React tree, not DOM tree

---

### **23. Explain Express static file serving and template engines.**

**Answer:** (5 marks)
```javascript
// Serve static files
app.use(express.static('public'));

// Template engine setup (EJS)
app.set('view engine', 'ejs');
app.set('views', './views');

// Render template
app.get('/', (req, res) => {
  res.render('index', { title: 'Home' });
});

// EJS template
// <h1><%= title %></h1>
```
- **Static Files**: CSS, JS, images from public folder
- **Template Engines**: EJS, Pug, Handlebars
- **When to use**: Server-side rendering, simple pages

---

### **24. What is memoization in React? Explain useMemo and useCallback.**

**Answer:** (5 marks)
```javascript
// useMemo: Memoize expensive calculations
const memoizedValue = useMemo(() => 
  computeExpensiveValue(a, b), [a, b]);

// useCallback: Memoize functions
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

// React.memo: Memoize components
const MemoizedComponent = React.memo(MyComponent);
```
- **Purpose**: Prevent unnecessary re-calculations/re-renders
- **When to use**: Expensive computations, callback props, pure components
- **Dependency Array**: Controls when to re-compute

---

### **25. Explain MongoDB Atlas and its features.**

**Answer:** (5 marks)
- **Cloud Database**: Fully managed MongoDB service
- **Features**:
  1. Automated backups and restores
  2. Monitoring and alerts
  3. Automatic scaling
  4. Security: Encryption, VPC peering
  5. Global clusters for low latency
- **Tiers**: Free (512MB), Shared, Dedicated
- **Setup**: 
  1. Create cluster
  2. Whitelist IP
  3. Create user
  4. Get connection string
- **Advantages**: No ops overhead, high availability, easy scaling

---

### **26. What are React Higher-Order Components? Explain with example.**

**Answer:** (5 marks)
```javascript
// HOC that adds loading functionality
const withLoading = (WrappedComponent) => {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) return <Spinner />;
    return <WrappedComponent {...props} />;
  };
};

// Usage
const UserListWithLoading = withLoading(UserList);
<UserListWithLoading isLoading={true} users={users} />
```
- **Purpose**: Reuse component logic, cross-cutting concerns
- **Pattern**: Function that takes component, returns enhanced component
- **Use Cases**: Authentication, logging, loading states
- **Alternatives**: Render props, custom hooks

---

### **27. Explain Express middleware chaining and error handling.**

**Answer:** (5 marks)
```javascript
// Middleware chaining
app.use(morgan('dev')); // Logging
app.use(express.json()); // Body parsing
app.use(cors()); // CORS
app.use('/api', apiRouter); // Routes
app.use(notFound); // 404 handler
app.use(errorHandler); // Error handler

// Error handling middleware
const errorHandler = (err, req, res, next) => {
  const status = err.status || 500;
  res.status(status).json({
    error: err.message,
    stack: process.env.NODE_ENV === 'production' ? null : err.stack
  });
};

// Order matters: error handlers last
```
- **Execution Order**: Sequential based on `app.use()` order
- `next()`: Pass to next middleware
- `next(err)`: Skip to error handler

---

### **28. What are React Refs? Explain forwardRef with example.**

**Answer:** (5 marks)
```javascript
// Creating ref
const inputRef = useRef();

// Using ref
<input ref={inputRef} />

// Accessing
inputRef.current.focus();

// forwardRef: Passing ref through components
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="fancy">
    {props.children}
  </button>
));

// Usage
const ref = useRef();
<FancyButton ref={ref}>Click</FancyButton>
```
- **Use Cases**: DOM access, focus management, animations
- **Imperative vs Declarative**: Refs are imperative escape hatch

---

### **29. Explain MongoDB data modeling for relationships.**

**Answer:** (5 marks)
1. **Embedding**: Document contains nested sub-documents
   ```javascript
   // User with embedded addresses
   {
     _id: 1,
     name: "John",
     addresses: [
       { street: "123 Main", city: "NYC" }
     ]
   }
   ```
   - **When**: One-to-few, data accessed together
   
2. **Referencing**: Documents reference each other by ID
   ```javascript
   // User referencing posts
   {
     _id: 1,
     name: "John",
     postIds: [101, 102]
   }
   ```
   - **When**: One-to-many, many-to-many, large arrays

3. **Hybrid**: Embed with critical info, reference for details

---

### **30. Explain React performance optimization techniques.**

**Answer:** (5 marks)
1. **useMemo/useCallback**: Memoize values/functions
2. **React.memo**: Prevent unnecessary re-renders
3. **Code Splitting**: Lazy loading components
4. **Virtualization**: For large lists
5. **Debouncing/Throttling**: For frequent events
6. **Production Build**: `npm run build` with minification
7. **Chunk Analysis**: `source-map-explorer`
8. **Webpack Optimization**: Tree shaking, compression
9. **PureComponent**: For class components
10. **Avoid Inline Functions/Objects**: In render methods

---

**Marking Scheme for 5-mark questions:**
- **5 marks**: Complete, accurate explanation with examples
- **4 marks**: Good explanation, minor omissions
- **3 marks**: Basic understanding, some errors
- **2 marks**: Partial knowledge, significant gaps
- **1 mark**: Minimal relevant information
- **0 marks**: Completely incorrect/no answer
