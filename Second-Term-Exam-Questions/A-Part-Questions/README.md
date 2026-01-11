# PART A Questions

## React Basics (Questions 1-25)
1. Which of the following is used to create a React component?
   a) function
   b) class
   c) both (a) and (b) ✓
   d) none of these

2. React is primarily used for building:
   a) Databases
   b) User interfaces ✓
   c) Backend servers
   d) Mobile operating systems

3. What does JSX stand for?
   a) JavaScript XHTML
   b) JavaScript XML ✓
   c) Java Syntax Extension
   d) JSON Extended Syntax

4. JSX allows you to:
   a) Write HTML-like syntax in JavaScript ✓
   b) Write JavaScript in HTML
   c) Create CSS styles
   d) Write SQL queries

5. What is the correct way to render a React component?
   a) ReactDOM.createComponent()
   b) ReactDOM.render() ✓
   c) ReactDOM.mount()
   d) ReactDOM.append()

6. In React, components must return:
   a) A single parent element ✓
   b) Multiple elements
   c) Only strings
   d) Only numbers

7. Which method is used to create a React app?
   a) npm install react
   b) npx create-react-app myapp ✓
   c) npm react-start
   d) npx react-start

8. React components are:
   a) Reusable and independent ✓
   b) Static and unchangeable
   c) Only for styling
   d) Dependent on jQuery

9. What is the purpose of Babel in React?
   a) To compile JSX to JavaScript ✓
   b) To create CSS styles
   c) To manage state
   d) To handle routing

10. Which of these is a valid React component?
    ```jsx
    a) function MyComponent() { return <div>Hello</div>; } ✓
    b) function MyComponent() { return "Hello"; }
    c) const MyComponent = "Hello";
    d) const MyComponent = () => Hello;
    ```

## React Hooks (Questions 26-45)
11. In React, which hook is used to handle side effects like API calls?
    a) useEffect() ✓
    b) useState()
    c) useRef()
    d) useContext()

12. Which hook is used to manage state in functional components?
    a) useEffect()
    b) useState() ✓
    c) useContext()
    d) useReducer()

13. The useState hook returns:
    a) Only the state value
    b) A state value and its setter function ✓
    c) Only the setter function
    d) An object with multiple states

14. useEffect hook with empty dependency array runs:
    a) On every render
    b) Only on initial render ✓
    c) Only when props change
    d) Never runs

15. Which hook is used to access context in functional components?
    a) useContext() ✓
    b) useContextAPI()
    c) useContextProvider()
    d) createContext()

16. The useRef hook is used to:
    a) Manage state
    b) Access DOM elements directly ✓
    c) Handle API calls
    d) Create context

17. Custom hooks in React:
    a) Must start with "use" ✓
    b) Must start with "hook"
    c) Cannot use other hooks
    d) Are only for class components

18. Which hook is used for performance optimization?
    a) useMemo() ✓
    b) useOptimize()
    c) usePerformance()
    d) useCallback()

19. useCallback hook memoizes:
    a) Values
    b) Functions ✓
    c) Components
    d) Styles

20. Rules of Hooks include:
    a) Only call hooks at the top level ✓
    b) Call hooks conditionally
    c) Call hooks in loops
    d) Call hooks in regular JavaScript functions

## Props & State Management (Questions 46-65)
21. What is the correct way to pass data from a parent component to a child component?
    a) Using state
    b) Using props ✓
    c) Using context
    d) Using useRef

22. Props in React are:
    a) Mutable
    b) Immutable ✓
    c) Only for functions
    d) Only for class components

23. State in React components is:
    a) Read-only
    b) Mutable using setState() ✓
    c) Passed from parent
    d) Only for functional components

24. What is "lifting state up" in React?
    a) Moving state to a common ancestor ✓
    b) Deleting state
    c) Converting state to props
    d) Storing state in localStorage

25. The key prop is used to:
    a) Style components
    b) Identify elements in lists uniquely ✓
    c) Pass event handlers
    d) Create refs

26. Controlled components in React:
    a) Manage their own state
    b) Have state controlled by React ✓
    c) Cannot have event handlers
    d) Are only class components

27. PropTypes are used to:
    a) Style components
    b) Validate component props ✓
    c) Create new components
    d) Handle events

28. Default props are set using:
    a) Component.defaultProps ✓
    b) Component.propsDefault
    c) defaultProps()
    d) setDefaultProps()

29. Children prop refers to:
    a) Child components passed between tags ✓
    b) Nested state
    c) Event handlers
    d) Context providers

30. What is prop drilling?
    a) Passing props through multiple levels ✓
    b) Drilling into prop types
    c) Creating props dynamically
    d) Deleting props

## React Router (Questions 66-85)
31. Which React Router component is used for navigation links?
    a) <a>
    b) <Link> ✓
    c) <RouterLink>
    d) <Nav>

32. React Router's <Route> component is used to:
    a) Style components
    b) Define navigation links
    c) Render components based on URL ✓
    d) Handle form submissions

33. Which component wraps the entire application in React Router?
    a) <BrowserRouter> ✓
    b) <RouterWrapper>
    c) <AppRouter>
    d) <RootRouter>

34. The useParams hook is used to:
    a) Access route parameters ✓
    b) Manage state
    c) Handle navigation
    d) Create routes

35. Which hook is used for programmatic navigation?
    a) useHistory() (v5) or useNavigate() (v6) ✓
    b) useNavigation()
    c) useRouter()
    d) useRedirect()

36. Nested routes are created using:
    a) <NestedRoute>
    b) <Route> inside other <Route> components ✓
    c) <ChildRoute>
    d) <SubRoute>

37. The exact prop in <Route> ensures:
    a) Route matches exactly the path ✓
    b) Route matches any path
    c) Route redirects automatically
    d) Route loads asynchronously

38. Which component is used for redirects?
    a) <Redirect> (v5) or <Navigate> (v6) ✓
    b) <GoTo>
    c) <SwitchRoute>
    d) <ChangeRoute>

39. The Switch component (v5) or Routes (v6):
    a) Renders the first matching route ✓
    b) Renders all matching routes
    c) Creates navigation menus
    d) Handles errors

40. Lazy loading routes uses:
    a) React.lazy() with Suspense ✓
    b) LoadRoute()
    c) AsyncRoute()
    d) DynamicImport()

## Redux Fundamentals (Questions 86-105)
41. Redux uses _____ to describe how the state changes.
    a) Components
    b) Reducers
    c) Actions ✓
    d) Store

42. In Redux, the store can be created using:
    a) createApp()
    b) createStore() ✓
    c) createReducer()
    d) useReducer()

43. Which of the following is the correct order of Redux data flow?
    a) Action → Reducer → Store → View ✓
    b) View → Store → Reducer → Action
    c) Store → View → Action → Reducer
    d) Reducer → Action → View → Store

44. Actions in Redux are:
    a) Plain JavaScript objects with type property ✓
    b) Functions that update state directly
    c) React components
    d) CSS classes

45. Action creators are:
    a) Functions that return actions ✓
    b) Components that create state
    c) Reducers that create actions
    d) Middleware functions

46. Reducers in Redux must be:
    a) Pure functions ✓
    b) Async functions
    c) Class methods
    d) Event handlers

47. The combineReducers function is used to:
    a) Combine multiple reducers ✓
    b) Combine actions
    c) Combine components
    d) Combine middleware

48. Middleware in Redux is used for:
    a) Handling side effects ✓
    b) Creating components
    c) Styling applications
    d) Routing

49. Redux Thunk middleware allows:
    a) Async actions ✓
    b) Synchronous actions only
    c) Direct state mutation
    d) Automatic routing

50. Redux Toolkit simplifies Redux by providing:
    a) configureStore() and createSlice() ✓
    b) createComponent()
    c) createActionOnly()
    d) simplifyRedux()

## Redux Advanced & React-Redux (Questions 106-125)
51. React-Redux provides:
    a) <Provider> component ✓
    b) <ReduxProvider>
    c) <StoreProvider>
    d) <StateProvider>

52. The useSelector hook is used to:
    a) Access Redux state in components ✓
    b) Create actions
    c) Dispatch actions
    d) Create reducers

53. The useDispatch hook returns:
    a) The Redux store
    b) Dispatch function ✓
    c) Current state
    d) Action creators

54. Immutable updates in Redux mean:
    a) State is never mutated directly ✓
    b) State is always mutated
    c) State is deleted
    d) State is converted to props

55. Redux DevTools are used for:
    a) Debugging Redux state changes ✓
    b) Styling components
    c) Creating components
    d) Handling routing

56. Normalized state shape in Redux means:
    a) Storing data in flat structure by ID ✓
    b) Storing nested objects
    c) Storing only strings
    d) Storing in localStorage

57. Redux Persist is used for:
    a) Persisting Redux state ✓
    b) Creating persistent components
    c) Styling persistence
    d) Routing persistence

58. Redux Saga is an alternative to:
    a) Redux Thunk for side effects ✓
    b) React Router
    c) React components
    d) CSS frameworks

59. Selectors in Redux are used to:
    a) Compute derived data from state ✓
    b) Create actions
    c) Dispatch actions
    d) Style components

60. Memoized selectors improve:
    a) Performance by caching results ✓
    b) Styling speed
    c) Component creation
    d) Routing speed

## Styling & CSS (Questions 126-145)
61. What is the main advantage of using Tailwind CSS in React projects?
    a) Predefined color themes only
    b) Utility-first classes for rapid styling ✓
    c) It replaces JavaScript logic
    d) It works only with Redux

62. CSS Modules in React:
    a) Scope CSS to components ✓
    b) Create global CSS only
    c) Replace JavaScript
    d) Handle state management

63. Styled-components library allows:
    a) Writing CSS in JavaScript ✓
    b) Writing JavaScript in CSS
    c) Creating Redux stores
    d) Handling routing

64. Inline styles in React are passed as:
    a) Strings
    b) JavaScript objects ✓
    c) Arrays
    d) Functions

65. CSS-in-JS benefits include:
    a) Scoped styles and dynamic styling ✓
    b) Better SEO
    c) Faster server rendering
    d) Smaller bundle size

66. Emotion is:
    a) A CSS-in-JS library ✓
    b) A state management library
    c) A routing library
    d) A testing library

67. PostCSS is used for:
    a) Transforming CSS with JavaScript ✓
    b) Creating React components
    c) Managing state
    d) Handling routing

68. PurgeCSS is used to:
    a) Remove unused CSS ✓
    b) Add CSS automatically
    c) Create CSS animations
    d) Manage CSS state

69. Sass/SCSS in React projects:
    a) Provides CSS preprocessor features ✓
    b) Replaces JavaScript
    c) Handles routing
    d) Manages state

70. Bootstrap with React:
    a) Provides pre-styled components ✓
    b) Replaces React
    c) Manages state
    d) Handles routing

## Testing & Optimization (Questions 146-165)
71. Jest is primarily used for:
    a) Testing JavaScript code ✓
    b) Styling components
    c) State management
    d) Routing

72. React Testing Library encourages testing:
    a) Implementation details
    b) User behavior ✓
    c) Internal state
    d) Private methods

73. Which method is used to test components?
    a) render() from React Testing Library ✓
    b) testComponent()
    c) checkComponent()
    d) verifyComponent()

74. The act() function is used to:
    a) Prepare components for assertions ✓
    b) Style components
    c) Create components
    d) Delete components

75. Code splitting in React improves:
    a) Performance by lazy loading ✓
    b) Styling speed
    c) State management
    d) Routing speed

76. React.memo() is used for:
    a) Preventing unnecessary re-renders ✓
    b) Creating memoized selectors
    c) Managing state
    d) Handling events

77. Webpack in React is used for:
    a) Module bundling ✓
    b) State management
    c) Routing
    d) Styling

78. Babel in React transpiles:
    a) ES6+ to older JavaScript ✓
    b) CSS to JavaScript
    c) HTML to JavaScript
    d) Images to components

79. Tree shaking removes:
    a) Unused code from bundle ✓
    b) Used code from bundle
    c) CSS styles
    d) React components

80. Lighthouse is used for:
    a) Auditing web performance ✓
    b) Creating components
    c) Managing state
    d) Handling routing

## Advanced React Concepts (Questions 166-185)
81. Error boundaries in React are:
    a) Components that catch JavaScript errors ✓
    b) Functions that throw errors
    c) CSS classes for errors
    d) Routing error handlers

82. Portals in React are used to:
    a) Render children outside DOM hierarchy ✓
    b) Create portals between pages
    c) Style components
    d) Manage state

83. React Fragments allow:
    a) Grouping elements without extra nodes ✓
    b) Creating fragments of state
    c) Breaking components
    d) Splitting CSS

84. Higher-Order Components (HOCs) are:
    a) Functions that take components and return new components ✓
    b) Higher order state
    c) Advanced CSS
    d) Complex routes

85. Render props pattern involves:
    a) Passing a function as prop to share code ✓
    b) Rendering props only
    c) Creating prop renderers
    d) Styling props

86. Context API is used for:
    a) Global state without prop drilling ✓
    b) Local state management
    c) Routing only
    d) Styling only

87. Compound components are:
    a) Components that work together ✓
    b) Broken components
    c) Simple components
    d) Styled components

88. React.forwardRef() is used to:
    a) Pass refs to child components ✓
    b) Create forward state
    c) Style refs
    d) Route refs

89. useImperativeHandle hook is used to:
    a) Customize instance value exposed with ref ✓
    b) Create imperative components
    c) Handle imperative state
    d) Style imperative elements

90. React.StrictMode helps with:
    a) Identifying potential problems ✓
    b) Creating strict components
    c) Strict styling
    d) Strict routing

## Deployment & Build Tools (Questions 186-200)
91. npm run build creates:
    a) Production-ready build ✓
    b) Development server
    c) Test environment
    d) Documentation

92. Environment variables in Create React App are prefixed with:
    a) REACT_APP_ ✓
    b) CRA_
    c) REACT_
    d) APP_

93. Netlify and Vercel are used for:
    a) Deploying React applications ✓
    b) Creating components
    c) Managing state
    d) Testing

94. Service workers in Create React App enable:
    a) Progressive Web App features ✓
    b) State management
    c) Routing
    d) Styling

95. The manifest.json file is for:
    a) PWA configuration ✓
    b) State configuration
    c) Route configuration
    d) Style configuration

96. Polyfills are used to:
    a) Support older browsers ✓
    b) Create new components
    c) Manage modern state
    d) Handle routing

97. Source maps help with:
    a) Debugging production code ✓
    b) Creating source components
    c) Managing source state
    d) Routing sources

98. CDN stands for:
    a) Content Delivery Network ✓
    b) Component Delivery Network
    c) Code Delivery Network
    d) CSS Delivery Network

99. Tree shaking is performed by:
    a) Webpack and Rollup ✓
    b) Babel only
    c) React only
    d) Redux only

100. The public folder in Create React App contains:
     a) Static assets ✓
     b) Source code
     c) State management
     d) Routing logic

---
