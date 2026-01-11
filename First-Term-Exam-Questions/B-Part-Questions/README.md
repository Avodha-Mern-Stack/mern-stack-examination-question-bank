# **Part B Questions**

## **JavaScript Fundamentals**

### 1. What is hoisting in JavaScript? Explain with an example.
**Answer:** Hoisting is JavaScript's default behavior of moving declarations to the top of their scope before code execution. Only declarations are hoisted, not initializations.

**Example:**
```javascript
console.log(x); // Output: undefined (not ReferenceError)
var x = 5;

// Above code is interpreted as:
var x;
console.log(x); // undefined
x = 5;

// Function declarations are fully hoisted:
sayHello(); // Output: "Hello"
function sayHello() {
    console.log("Hello");
}

// But function expressions are not:
sayHi(); // TypeError: sayHi is not a function
var sayHi = function() {
    console.log("Hi");
};
```

### 2. What are JavaScript closures? Explain how they work with a simple example.
**Answer:** A closure is a function that has access to its own scope, the outer function's scope, and the global scope, even after the outer function has finished execution. Closures "remember" the environment in which they were created.

**Example:**
```javascript
function outerFunction(outerVariable) {
    return function innerFunction(innerVariable) {
        console.log('Outer variable:', outerVariable);
        console.log('Inner variable:', innerVariable);
    };
}

const newFunction = outerFunction('outside');
newFunction('inside');
// Output:
// Outer variable: outside
// Inner variable: inside
// Even though outerFunction has finished executing, 
// innerFunction still remembers outerVariable
```

### 3. What is the difference between synchronous and asynchronous JavaScript? Give examples.
**Answer:** 
- **Synchronous:** Code executes line by line, each operation waits for the previous one to complete
- **Asynchronous:** Operations can run in parallel; code doesn't wait for one operation to complete before moving to the next

**Examples:**
```javascript
// Synchronous example
console.log('First');
console.log('Second'); // Waits for first to complete
console.log('Third');  // Waits for second to complete

// Asynchronous example
console.log('First');
setTimeout(() => {
    console.log('Second'); // Executes after 2 seconds
}, 2000);
console.log('Third'); // Executes immediately, doesn't wait for setTimeout
```

### 4. Explain null and undefined in JavaScript. How are they different?
**Answer:**
- **undefined:** A variable that has been declared but not assigned a value
- **null:** An assignment value that represents no value or no object

**Differences:**
```javascript
let a;          // undefined - declared but not assigned
let b = null;   // null - explicitly assigned null value

console.log(typeof a);    // "undefined"
console.log(typeof b);    // "object" (historical bug in JavaScript)

console.log(a == b);      // true (loose equality)
console.log(a === b);     // false (strict equality)

// Common use cases:
function doSomething(param) {
    if (param === undefined) {
        // Parameter was not provided
    }
    if (param === null) {
        // Parameter was explicitly set to null
    }
}
```

### 5. What is event bubbling and event capturing in JavaScript?
**Answer:** Event propagation in the DOM occurs in two phases:
- **Event Capturing (Trickling):** Event moves from root to target element (top-down)
- **Event Bubbling:** Event moves from target element to root (bottom-up)

**Example:**
```html
<div id="grandparent">
    <div id="parent">
        <button id="child">Click me</button>
    </div>
</div>

<script>
// Capturing phase
document.getElementById('grandparent').addEventListener('click', () => {
    console.log('Grandparent captured');
}, true); // true enables capturing phase

// Bubbling phase (default)
document.getElementById('parent').addEventListener('click', () => {
    console.log('Parent bubbled');
});

document.getElementById('child').addEventListener('click', () => {
    console.log('Child clicked');
});
</script>
```

### 6. Explain the 'this' keyword in JavaScript with different contexts.
**Answer:** The value of 'this' depends on how a function is called:
```javascript
// 1. Global context
console.log(this); // Window (browser) / Global (Node.js)

// 2. Object method
const obj = {
    name: 'John',
    greet() {
        console.log(this.name); // 'this' refers to obj
    }
};

// 3. Constructor function
function Person(name) {
    this.name = name; // 'this' refers to new instance
}
const person = new Person('Alice');

// 4. Arrow function (lexical 'this')
const obj2 = {
    name: 'Bob',
    greet: () => {
        console.log(this.name); // 'this' refers to outer scope
    }
};

// 5. Event handler
button.addEventListener('click', function() {
    console.log(this); // 'this' refers to button element
});
```

### 7. What are promises in JavaScript? Explain with an example.
**Answer:** Promises are objects representing the eventual completion or failure of an asynchronous operation.

**Example:**
```javascript
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
            resolve('Operation successful');
        } else {
            reject('Operation failed');
        }
    }, 1000);
});

// Using the promise
myPromise
    .then(result => {
        console.log('Success:', result);
    })
    .catch(error => {
        console.log('Error:', error);
    })
    .finally(() => {
        console.log('Operation completed');
    });
```

### 8. What is the difference between == and === operators?
**Answer:**
- **== (Loose equality):** Compares values after type coercion
- **=== (Strict equality):** Compares values without type coercion

**Examples:**
```javascript
console.log(5 == '5');    // true (type coercion)
console.log(5 === '5');   // false (different types)

console.log(0 == false);  // true
console.log(0 === false); // false

console.log(null == undefined);  // true
console.log(null === undefined); // false

console.log(NaN == NaN);  // false
console.log(NaN === NaN); // false
```

### 9. Explain call, apply, and bind methods with examples.
**Answer:** These methods allow you to set the value of 'this' for a function.

**Examples:**
```javascript
const person = {
    name: 'Alice',
    greet(greeting, punctuation) {
        console.log(`${greeting}, ${this.name}${punctuation}`);
    }
};

const anotherPerson = { name: 'Bob' };

// call() - immediately invokes with given 'this' and arguments
person.greet.call(anotherPerson, 'Hello', '!');

// apply() - similar to call() but accepts arguments as array
person.greet.apply(anotherPerson, ['Hi', '!!']);

// bind() - returns a new function with bound 'this' value
const boundGreet = person.greet.bind(anotherPerson);
boundGreet('Hey', '!!!');
```

### 10. What are template literals in ES6? Explain their advantages.
**Answer:** Template literals are string literals allowing embedded expressions, multi-line strings, and string interpolation.

**Advantages and Examples:**
```javascript
// 1. String interpolation
const name = 'John';
const age = 30;
console.log(`My name is ${name} and I am ${age} years old`);

// 2. Multi-line strings
const multiLine = `This is a 
multi-line 
string`;

// 3. Expression evaluation
console.log(`The sum is ${5 + 3}`); // "The sum is 8"

// 4. Tagged templates
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => 
        `${result}${str}<strong>${values[i] || ''}</strong>`, '');
}

const user = 'Alice';
const score = 95;
console.log(highlight`User ${user} scored ${score} points`);
```

### 11. What are arrow functions? How do they differ from regular functions?
**Answer:** Arrow functions are a concise syntax for writing function expressions in ES6.

**Differences:**
```javascript
// Syntax differences
const regularFunc = function(x) { return x * 2; };
const arrowFunc = (x) => x * 2;

// No 'this' binding (lexical 'this')
const obj = {
    value: 10,
    regular: function() {
        console.log(this.value); // 10
    },
    arrow: () => {
        console.log(this.value); // undefined (refers to outer scope)
    }
};

// Cannot be used as constructors
const Arrow = () => {};
// new Arrow(); // TypeError: Arrow is not a constructor

// No arguments object
function regular() { console.log(arguments); }
const arrow = () => { console.log(arguments); }; // ReferenceError
```

### 12. Explain the spread and rest operators with examples.
**Answer:** Both use `...` syntax but serve different purposes.

**Examples:**
```javascript
// Spread operator (expands)
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 }; // { a: 1, b: 2, c: 3 }

// Function calls
Math.max(...arr1); // Spreads array as arguments

// Rest operator (collects)
function sum(...numbers) { // Collects all arguments into array
    return numbers.reduce((total, num) => total + num, 0);
}
sum(1, 2, 3, 4); // 10

// Array destructuring
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest);  // [2, 3, 4]
```

### 13. What is destructuring in JavaScript? Provide examples.
**Answer:** Destructuring allows extracting values from arrays or properties from objects into distinct variables.

**Examples:**
```javascript
// Array destructuring
const colors = ['red', 'green', 'blue'];
const [firstColor, secondColor] = colors;
console.log(firstColor);  // 'red'
console.log(secondColor); // 'green'

// Skipping elements
const [,, thirdColor] = colors;
console.log(thirdColor); // 'blue'

// Object destructuring
const person = { name: 'John', age: 30, city: 'NYC' };
const { name, age } = person;
console.log(name); // 'John'
console.log(age);  // 30

// Renaming variables
const { city: location } = person;
console.log(location); // 'NYC'

// Default values
const { country = 'USA' } = person;
console.log(country); // 'USA'
```

### 14. What are JavaScript modules? Explain export and import.
**Answer:** Modules allow splitting code into separate files with private and public parts.

**Examples:**
```javascript
// mathUtils.js - Exporting
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export default function multiply(a, b) {
    return a * b;
}

// app.js - Importing
import multiply, { PI, add } from './mathUtils.js';

console.log(PI);           // 3.14159
console.log(add(2, 3));    // 5
console.log(multiply(2, 3)); // 6

// Import everything
import * as math from './mathUtils.js';
console.log(math.PI);
console.log(math.add(2, 3));
```

### 15. Explain async/await with an example.
**Answer:** Async/await is syntactic sugar over promises, making asynchronous code look synchronous.

**Example:**
```javascript
// Traditional promise approach
function fetchData() {
    return fetch('https://api.example.com/data')
        .then(response => response.json())
        .then(data => {
            console.log(data);
            return data;
        })
        .catch(error => {
            console.error('Error:', error);
        });
}

// Async/await approach
async function fetchDataAsync() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}

// Multiple async operations
async function fetchMultiple() {
    const [data1, data2] = await Promise.all([
        fetch('https://api.example.com/data1').then(r => r.json()),
        fetch('https://api.example.com/data2').then(r => r.json())
    ]);
    return { data1, data2 };
}
```

### 16. What is memoization? Provide a JavaScript implementation.
**Answer:** Memoization is an optimization technique that stores results of expensive function calls.

**Implementation:**
```javascript
function memoize(fn) {
    const cache = new Map();
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            console.log('Returning cached result');
            return cache.get(key);
        }
        
        console.log('Calculating result');
        const result = fn(...args);
        cache.set(key, result);
        return result;
    };
}

// Example usage
function expensiveCalculation(n) {
    let result = 0;
    for (let i = 0; i < n * 1000000; i++) {
        result += i;
    }
    return result;
}

const memoizedCalc = memoize(expensiveCalculation);

console.log(memoizedCalc(5)); // Calculates
console.log(memoizedCalc(5)); // Returns cached
console.log(memoizedCalc(5)); // Returns cached
```

### 17. What is event delegation? Explain with an example.
**Answer:** Event delegation attaches a single event listener to a parent element instead of multiple listeners to child elements.

**Example:**
```html
<ul id="itemList">
    <li data-id="1">Item 1</li>
    <li data-id="2">Item 2</li>
    <li data-id="3">Item 3</li>
    <li data-id="4">Item 4</li>
</ul>

<script>
// Without delegation (inefficient)
const items = document.querySelectorAll('#itemList li');
items.forEach(item => {
    item.addEventListener('click', function() {
        console.log('Clicked:', this.dataset.id);
    });
});

// With delegation (efficient)
document.getElementById('itemList').addEventListener('click', function(event) {
    if (event.target.tagName === 'LI') {
        console.log('Clicked:', event.target.dataset.id);
    }
});

// Works even for dynamically added items
setTimeout(() => {
    const newItem = document.createElement('li');
    newItem.textContent = 'Item 5';
    newItem.dataset.id = '5';
    document.getElementById('itemList').appendChild(newItem);
    // Clicking newItem still triggers the event
}, 2000);
</script>
```

### 18. Explain the concept of prototypes in JavaScript.
**Answer:** JavaScript uses prototypal inheritance where objects can inherit properties from other objects.

**Examples:**
```javascript
// Constructor function
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// Adding method to prototype
Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

// Creating instance
const john = new Person('John', 30);
console.log(john.greet()); // "Hello, I'm John"

// Prototype chain
console.log(john.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null

// ES6 Class syntax (syntactic sugar)
class PersonClass {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
}
```

### 19. What is the difference between localStorage, sessionStorage, and cookies?
**Answer:** 

| Feature | localStorage | sessionStorage | Cookies |
|---------|-------------|----------------|---------|
| Capacity | 5-10MB | 5-10MB | 4KB |
| Expiration | Never | Tab close | Manual set |
| Server Access | No | No | Yes (with each request) |
| Scope | Origin | Tab | Domain/Path |
| API | Simple key-value | Simple key-value | String |

**Examples:**
```javascript
// localStorage
localStorage.setItem('username', 'john');
console.log(localStorage.getItem('username')); // 'john'
localStorage.removeItem('username');

// sessionStorage
sessionStorage.setItem('token', 'abc123');
console.log(sessionStorage.getItem('token'));

// Cookies
document.cookie = "username=John; expires=Fri, 31 Dec 2024 23:59:59 GMT; path=/";
console.log(document.cookie);
```

### 20. Explain CORS (Cross-Origin Resource Sharing).
**Answer:** CORS is a security mechanism that allows restricted resources on a web page to be requested from another domain.

**How it works:**
```javascript
// Server response headers needed:
// Access-Control-Allow-Origin: https://example.com
// Access-Control-Allow-Methods: GET, POST, PUT
// Access-Control-Allow-Headers: Content-Type

// Simple request
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data));

// Preflight request (for non-simple requests)
fetch('https://api.example.com/data', {
    method: 'PUT',
    headers: {
        'Content-Type': 'application/json',
        'X-Custom-Header': 'value'
    },
    body: JSON.stringify({ data: 'test' })
});

// Common CORS errors and solutions:
// 1. Add proper headers on server
// 2. Use proxy server
// 3. JSONP (for GET requests only)
// 4. Configure CORS in backend
```

### 21. What are Web Workers? When would you use them?
**Answer:** Web Workers allow running scripts in background threads separate from the main execution thread.

**Example:**
```javascript
// main.js
const worker = new Worker('worker.js');

worker.onmessage = function(event) {
    console.log('Result:', event.data);
};

worker.postMessage({ numbers: [1, 2, 3, 4, 5] });

// worker.js
self.onmessage = function(event) {
    const numbers = event.data.numbers;
    const sum = numbers.reduce((a, b) => a + b, 0);
    self.postMessage(sum);
};

// Use cases:
// 1. Complex calculations
// 2. Image/video processing
// 3. Large dataset manipulation
// 4. Real-time data processing
```

### 22. Explain the event loop in JavaScript.
**Answer:** The event loop is responsible for executing code, collecting and processing events, and executing queued sub-tasks.

**Visualization:**
```javascript
console.log('1: Start');

setTimeout(() => {
    console.log('2: Timeout callback');
}, 0);

Promise.resolve().then(() => {
    console.log('3: Promise resolved');
});

console.log('4: End');

// Output order:
// 1: Start
// 4: End
// 3: Promise resolved (microtask queue)
// 2: Timeout callback (callback queue)
```

### 23. What are generators in JavaScript? Provide an example.
**Answer:** Generators are functions that can be paused and resumed, using `function*` syntax and `yield` keyword.

**Example:**
```javascript
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
    return 4;
}

const gen = numberGenerator();

console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: 4, done: true }

// Practical example: Infinite sequence
function* infiniteSequence() {
    let i = 0;
    while (true) {
        yield i++;
    }
}

const infinite = infiniteSequence();
console.log(infinite.next().value); // 0
console.log(infinite.next().value); // 1

// Generator with parameters
function* fibonacci() {
    let a = 0, b = 1;
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}
```

### 24. What is the difference between var, let, and const?
**Answer:** 

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes (undefined) | Yes (TDZ) | Yes (TDZ) |
| Reassignment | Yes | Yes | No |
| Redeclaration | Yes | No | No |
| Temporal Dead Zone | No | Yes | Yes |

**Examples:**
```javascript
// Scope differences
function testScope() {
    if (true) {
        var x = 1;     // Function scoped
        let y = 2;     // Block scoped
        const z = 3;   // Block scoped
    }
    console.log(x); // 1
    // console.log(y); // ReferenceError
    // console.log(z); // ReferenceError
}

// Temporal Dead Zone
console.log(a); // undefined
var a = 1;

// console.log(b); // ReferenceError (TDZ)
let b = 2;

// Reassignment
var c = 1;
c = 2; // OK

let d = 1;
d = 2; // OK

const e = 1;
// e = 2; // TypeError
```

### 25. Explain JavaScript's execution context.
**Answer:** Execution context is the environment where JavaScript code is evaluated and executed.

**Types:**
1. **Global Execution Context**
2. **Function Execution Context**
3. **Eval Execution Context**

**Components:**
```javascript
// Example demonstrating execution context
var globalVar = 'global';

function outer() {
    var outerVar = 'outer';
    
    function inner() {
        var innerVar = 'inner';
        console.log(globalVar);  // 'global'
        console.log(outerVar);   // 'outer'
        console.log(innerVar);   // 'inner'
    }
    
    inner();
}

outer();

// Creation Phase (for each context):
// 1. Create Variable Object (VO)
// 2. Create scope chain
// 3. Determine 'this' value

// Execution Phase:
// 1. Variable assignment
// 2. Function execution
// 3. Reference resolution
```

### 26. What are JavaScript design patterns? Explain Singleton pattern.
**Answer:** Design patterns are reusable solutions to common problems in software design.

**Singleton Pattern Example:**
```javascript
class DatabaseConnection {
    constructor() {
        if (DatabaseConnection.instance) {
            return DatabaseConnection.instance;
        }
        
        this.connection = this.createConnection();
        DatabaseConnection.instance = this;
        
        return this;
    }
    
    createConnection() {
        // Simulate database connection
        console.log('Creating new database connection...');
        return { connected: true };
    }
    
    query(sql) {
        console.log(`Executing: ${sql}`);
        return { results: [] };
    }
}

// Usage
const db1 = new DatabaseConnection();
const db2 = new DatabaseConnection();

console.log(db1 === db2); // true (same instance)

// Alternative implementation
const ConfigManager = (function() {
    let instance;
    let config = {};
    
    function createInstance() {
        return {
            set: (key, value) => {
                config[key] = value;
            },
            get: (key) => {
                return config[key];
            }
        };
    }
    
    return {
        getInstance: function() {
            if (!instance) {
                instance = createInstance();
            }
            return instance;
        }
    };
})();
```

### 27. What is the Module Pattern in JavaScript?
**Answer:** The Module Pattern uses closures to create private and public members.

**Implementation:**
```javascript
const Calculator = (function() {
    // Private members
    let memory = 0;
    
    function validateNumber(num) {
        return typeof num === 'number' && !isNaN(num);
    }
    
    // Public interface
    return {
        add: function(a, b) {
            if (!validateNumber(a) || !validateNumber(b)) {
                throw new Error('Invalid numbers');
            }
            return a + b;
        },
        
        subtract: function(a, b) {
            return a - b;
        },
        
        storeInMemory: function(value) {
            memory = value;
        },
        
        recallMemory: function() {
            return memory;
        },
        
        clearMemory: function() {
            memory = 0;
        }
    };
})();

// Usage
console.log(Calculator.add(5, 3)); // 8
Calculator.storeInMemory(10);
console.log(Calculator.recallMemory()); // 10
// Cannot access validateNumber directly (private)
```

### 28. Explain the Observer Pattern in JavaScript.
**Answer:** The Observer Pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

**Implementation:**
```javascript
class Subject {
    constructor() {
        this.observers = [];
    }
    
    subscribe(observer) {
        this.observers.push(observer);
    }
    
    unsubscribe(observer) {
        this.observers = this.observers.filter(obs => obs !== observer);
    }
    
    notify(data) {
        this.observers.forEach(observer => observer.update(data));
    }
}

class Observer {
    constructor(name) {
        this.name = name;
    }
    
    update(data) {
        console.log(`${this.name} received:`, data);
    }
}

// Usage
const subject = new Subject();

const observer1 = new Observer('Observer 1');
const observer2 = new Observer('Observer 2');

subject.subscribe(observer1);
subject.subscribe(observer2);

subject.notify('Hello Observers!');
// Output:
// Observer 1 received: Hello Observers!
// Observer 2 received: Hello Observers!

subject.unsubscribe(observer1);
subject.notify('Second notification');
// Output: Observer 2 received: Second notification

// Practical example: Event System
class EventEmitter {
    constructor() {
        this.events = {};
    }
    
    on(event, listener) {
        if (!this.events[event]) {
            this.events[event] = [];
        }
        this.events[event].push(listener);
    }
    
    off(event, listener) {
        if (!this.events[event]) return;
        this.events[event] = this.events[event].filter(l => l !== listener);
    }
    
    emit(event, ...args) {
        if (!this.events[event]) return;
        this.events[event].forEach(listener => listener(...args));
    }
}
```

### 29. What is currying in JavaScript? Provide an example.
**Answer:** Currying transforms a function with multiple arguments into a sequence of functions with single arguments.

**Example:**
```javascript
// Regular function
function add(a, b, c) {
    return a + b + c;
}

// Curried version
function curryAdd(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

console.log(add(1, 2, 3));         // 6
console.log(curryAdd(1)(2)(3));    // 6

// Generic curry function
function curry(fn) {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        } else {
            return function(...args2) {
                return curried.apply(this, args.concat(args2));
            };
        }
    };
}

// Usage with generic curry
const curriedMultiply = curry((a, b, c) => a * b * c);
console.log(curriedMultiply(2)(3)(4)); // 24
console.log(curriedMultiply(2, 3)(4)); // 24
console.log(curriedMultiply(2, 3, 4)); // 24

// Practical example: Configuration
function createLogger(level) {
    return function(message) {
        return function(timestamp) {
            return `[${level}] ${timestamp}: ${message}`;
        };
    };
}

const errorLog = createLogger('ERROR');
const errorWithTime = errorLog('Database connection failed');
console.log(errorWithTime('2024-01-15 10:30:00'));
// [ERROR] 2024-01-15 10:30:00: Database connection failed
```

### 30. Explain the Factory Pattern in JavaScript.
**Answer:** The Factory Pattern creates objects without specifying the exact class of object that will be created.

**Example:**
```javascript
// Product classes
class Car {
    constructor(model, price) {
        this.model = model;
        this.price = price;
        this.type = 'car';
    }
    
    drive() {
        console.log(`Driving ${this.model}`);
    }
}

class Truck {
    constructor(model, price) {
        this.model = model;
        this.price = price;
        this.type = 'truck';
    }
    
    haul() {
        console.log(`Hauling with ${this.model}`);
    }
}

class Motorcycle {
    constructor(model, price) {
        this.model = model;
        this.price = price;
        this.type = 'motorcycle';
    }
    
    ride() {
        console.log(`Riding ${this.model}`);
    }
}

// Factory
class VehicleFactory {
    createVehicle(type, model, price) {
        switch(type.toLowerCase()) {
            case 'car':
                return new Car(model, price);
            case 'truck':
                return new Truck(model, price);
            case 'motorcycle':
                return new Motorcycle(model, price);
            default:
                throw new Error(`Unknown vehicle type: ${type}`);
        }
    }
}

// Usage
const factory = new VehicleFactory();

const sedan = factory.createVehicle('car', 'Sedan', 20000);
const pickup = factory.createVehicle('truck', 'Pickup', 35000);
const bike = factory.createVehicle('motorcycle', 'Sport Bike', 15000);

sedan.drive();      // Driving Sedan
pickup.haul();      // Hauling with Pickup
bike.ride();        // Riding Sport Bike

// Alternative: Static Factory
class UserFactory {
    static createUser(type, data) {
        const userTypes = {
            admin: AdminUser,
            customer: CustomerUser,
            guest: GuestUser
        };
        
        const UserClass = userTypes[type];
        if (!UserClass) {
            throw new Error(`Invalid user type: ${type}`);
        }
        
        return new UserClass(data);
    }
}
```

### 31. What is the Strategy Pattern? Provide an example.
**Answer:** The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable.

**Example:**
```javascript
// Payment strategies
class CreditCardPayment {
    constructor(cardNumber, expiry) {
        this.cardNumber = cardNumber;
        this.expiry = expiry;
    }
    
    pay(amount) {
        console.log(`Paid $${amount} using Credit Card ending in ${this.cardNumber.slice(-4)}`);
        return true;
    }
}

class PayPalPayment {
    constructor(email) {
        this.email = email;
    }
    
    pay(amount) {
        console.log(`Paid $${amount} using PayPal: ${this.email}`);
        return true;
    }
}

class CryptoPayment {
    constructor(walletAddress) {
        this.walletAddress = walletAddress;
    }
    
    pay(amount) {
        console.log(`Paid $${amount} using Crypto to ${this.walletAddress.slice(0, 8)}...`);
        return true;
    }
}

// Context
class ShoppingCart {
    constructor() {
        this.items = [];
        this.paymentStrategy = null;
    }
    
    addItem(item, price) {
        this.items.push({ item, price });
    }
    
    setPaymentStrategy(strategy) {
        this.paymentStrategy = strategy;
    }
    
    checkout() {
        const total = this.items.reduce((sum, item) => sum + item.price, 0);
        console.log(`Total: $${total}`);
        
        if (!this.paymentStrategy) {
            console.log('Please select a payment method');
            return false;
        }
        
        return this.paymentStrategy.pay(total);
    }
}

// Usage
const cart = new ShoppingCart();
cart.addItem('Laptop', 999);
cart.addItem('Mouse', 49);

// Select payment strategy dynamically
cart.setPaymentStrategy(new CreditCardPayment('4111111111111111', '12/25'));
cart.checkout();

cart.setPaymentStrategy(new PayPalPayment('user@example.com'));
cart.checkout();

// Another example: Sorting strategies
const sortingStrategies = {
    bubbleSort: (arr) => {
        console.log('Using Bubble Sort');
        return [...arr].sort((a, b) => a - b);
    },
    
    quickSort: (arr) => {
        console.log('Using Quick Sort');
        return [...arr].sort((a, b) => a - b);
    },
    
    mergeSort: (arr) => {
        console.log('Using Merge Sort');
        return [...arr].sort((a, b) => a - b);
    }
};

function sortArray(arr, strategy = 'quickSort') {
    const sorter = sortingStrategies[strategy];
    if (!sorter) {
        throw new Error(`Unknown sorting strategy: ${strategy}`);
    }
    return sorter(arr);
}
```

### 32. Explain the Decorator Pattern in JavaScript.
**Answer:** The Decorator Pattern adds new functionality to an existing object without altering its structure.

**Example:**
```javascript
// Base class
class Coffee {
    cost() {
        return 5;
    }
    
    description() {
        return 'Simple coffee';
    }
}

// Decorators
class MilkDecorator {
    constructor(coffee) {
        this.coffee = coffee;
    }
    
    cost() {
        return this.coffee.cost() + 2;
    }
    
    description() {
        return this.coffee.description() + ', with milk';
    }
}

class SugarDecorator {
    constructor(coffee) {
        this.coffee = coffee;
    }
    
    cost() {
        return this.coffee.cost() + 1;
    }
    
    description() {
        return this.coffee.description() + ', with sugar';
    }
}

class WhipCreamDecorator {
    constructor(coffee) {
        this.coffee = coffee;
    }
    
    cost() {
        return this.coffee.cost() + 3;
    }
    
    description() {
        return this.coffee.description() + ', with whip cream';
    }
}

// Usage
let myCoffee = new Coffee();
console.log(myCoffee.description(), '- $' + myCoffee.cost());

myCoffee = new MilkDecorator(myCoffee);
console.log(myCoffee.description(), '- $' + myCoffee.cost());

myCoffee = new SugarDecorator(myCoffee);
console.log(myCoffee.description(), '- $' + myCoffee.cost());

myCoffee = new WhipCreamDecorator(myCoffee);
console.log(myCoffee.description(), '- $' + myCoffee.cost());

// ES7 Decorators (with transpiler)
function log(target, name, descriptor) {
    const original = descriptor.value;
    
    descriptor.value = function(...args) {
        console.log(`Calling ${name} with arguments:`, args);
        const result = original.apply(this, args);
        console.log(`Result:`, result);
        return result;
    };
    
    return descriptor;
}

class Calculator {
    @log
    add(a, b) {
        return a + b;
    }
}
```

### 33. What is the difference between shallow copy and deep copy?
**Answer:** 
- **Shallow Copy:** Copies only the first level of properties
- **Deep Copy:** Copies all nested properties recursively

**Examples:**
```javascript
const original = {
    name: 'John',
    address: {
        city: 'NYC',
        zip: '10001'
    },
    hobbies: ['reading', 'gaming']
};

// Shallow copy methods
const shallow1 = Object.assign({}, original);
const shallow2 = { ...original };

// Deep copy methods
const deep1 = JSON.parse(JSON.stringify(original));
const deep2 = structuredClone(original); // Modern browsers

// Custom deep copy function
function deepCopy(obj) {
    if (obj === null || typeof obj !== 'object') {
        return obj;
    }
    
    if (obj instanceof Date) {
        return new Date(obj.getTime());
    }
    
    if (obj instanceof Array) {
        return obj.map(item => deepCopy(item));
    }
    
    if (typeof obj === 'object') {
        const copy = {};
        for (const key in obj) {
            if (obj.hasOwnProperty(key)) {
                copy[key] = deepCopy(obj[key]);
            }
        }
        return copy;
    }
}

// Testing the difference
original.address.city = 'LA';
original.hobbies.push('swimming');

console.log(shallow1.address.city); // 'LA' (affected)
console.log(deep1.address.city);    // 'NYC' (not affected)
```

### 34. Explain the concept of throttling and debouncing.
**Answer:** Both are techniques to limit the rate of function execution.

**Throttling:** Ensures a function is called at most once in a specified period
**Debouncing:** Ensures a function is called only after a certain period of inactivity

**Implementations:**
```javascript
// Throttle implementation
function throttle(func, limit) {
    let inThrottle;
    return function() {
        const args = arguments;
        const context = this;
        if (!inThrottle) {
            func.apply(context, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// Debounce implementation
function debounce(func, delay) {
    let timeoutId;
    return function() {
        const args = arguments;
        const context = this;
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(context, args), delay);
    };
}

// Usage examples
window.addEventListener('resize', throttle(function() {
    console.log('Resize handler - throttled');
}, 200));

const searchInput = document.getElementById('search');
searchInput.addEventListener('input', debounce(function() {
    console.log('Searching for:', this.value);
}, 300));

// Visual comparison
/*
Scenario: User types "hello" quickly

Debounce (300ms):
- User types 'h' - timer starts
- Types 'e' before 300ms - timer resets
- Types 'l' before 300ms - timer resets
- Types 'l' before 300ms - timer resets
- Types 'o' - waits 300ms - function executes once

Throttle (300ms):
- User types 'h' - function executes immediately
- Types 'e' before 300ms - ignored
- Types 'l' before 300ms - ignored
- Types 'l' before 300ms - ignored
- Types 'o' - if >300ms since last execution, executes again
*/
```

### 35. What are service workers? Explain their lifecycle.
**Answer:** Service workers are scripts that run in the background, separate from web pages, enabling features like push notifications and background sync.

**Lifecycle:**
1. **Registration** → 2. **Installation** → 3. **Activation** → 4. **Idle** → 5. **Termination** → 6. **Fetch/Message Events**

**Example:**
```javascript
// main.js - Registration
if ('serviceWorker' in navigator) {
    window.addEventListener('load', function() {
        navigator.serviceWorker.register('/sw.js')
            .then(function(registration) {
                console.log('ServiceWorker registration successful');
            })
            .catch(function(err) {
                console.log('ServiceWorker registration failed:', err);
            });
    });
}

// sw.js - Service Worker
const CACHE_NAME = 'my-cache-v1';
const urlsToCache = [
    '/',
    '/styles/main.css',
    '/script/main.js'
];

// Installation
self.addEventListener('install', event => {
    event.waitUntil(
        caches.open(CACHE_NAME)
            .then(cache => {
                console.log('Opened cache');
                return cache.addAll(urlsToCache);
            })
    );
});

// Activation
self.addEventListener('activate', event => {
    event.waitUntil(
        caches.keys().then(cacheNames => {
            return Promise.all(
                cacheNames.map(cacheName => {
                    if (cacheName !== CACHE_NAME) {
                        console.log('Deleting old cache:', cacheName);
                        return caches.delete(cacheName);
                    }
                })
            );
        })
    );
});

// Fetch events
self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request)
            .then(response => {
                // Cache hit - return response
                if (response) {
                    return response;
                }
                
                // Clone the request
                const fetchRequest = event.request.clone();
                
                return fetch(fetchRequest).then(response => {
                    // Check if valid response
                    if (!response || response.status !== 200 || 
                        response.type !== 'basic') {
                        return response;
                    }
                    
                    // Clone the response
                    const responseToCache = response.clone();
                    
                    caches.open(CACHE_NAME)
                        .then(cache => {
                            cache.put(event.request, responseToCache);
                        });
                    
                    return response;
                });
            })
    );
});

// Message handling
self.addEventListener('message', event => {
    if (event.data.action === 'skipWaiting') {
        self.skipWaiting();
    }
});

// Push notifications
self.addEventListener('push', event => {
    const title = 'Push Notification';
    const options = {
        body: event.data.text(),
        icon: '/icon.png',
        badge: '/badge.png'
    };
    
    event.waitUntil(
        self.registration.showNotification(title, options)
    );
});
```

### 36. Explain the concept of progressive web apps (PWAs).
**Answer:** PWAs are web applications that use modern web capabilities to deliver an app-like experience.

**Core PWA Features:**
1. **Reliable** - Load instantly with service workers
2. **Fast** - Respond quickly to user interactions
3. **Engaging** - Feel like a native app

**Essential Components:**
```html
<!-- Manifest file -->
<link rel="manifest" href="/manifest.json">

<!-- Service worker registration -->
<script>
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js');
}
</script>
```

**manifest.json:**
```json
{
    "name": "My PWA",
    "short_name": "PWA",
    "start_url": "/",
    "display": "standalone",
    "background_color": "#ffffff",
    "theme_color": "#000000",
    "icons": [
        {
            "src": "/icon-192.png",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "/icon-512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ]
}
```

### 37. What are WebSockets? How do they differ from HTTP?
**Answer:** WebSockets provide full-duplex communication channels over a single TCP connection.

**Comparison:**
| Feature | HTTP | WebSocket |
|---------|------|-----------|
| Connection | Stateless, closes after response | Persistent, stays open |
| Communication | Request-response | Bi-directional |
| Header overhead | High (per request) | Low (once during handshake) |
| Real-time | Poor (polling needed) | Excellent |
| Use case | Document transfer | Real-time apps |

**WebSocket Example:**
```javascript
// Client-side
const socket = new WebSocket('wss://example.com/socket');

socket.onopen = function(event) {
    console.log('Connection established');
    socket.send('Hello Server!');
};

socket.onmessage = function(event) {
    console.log('Message from server:', event.data);
};

socket.onerror = function(error) {
    console.log('WebSocket Error:', error);
};

socket.onclose = function(event) {
    console.log('Connection closed:', event.code, event.reason);
};

// Server-side (Node.js example)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', function connection(ws) {
    console.log('New client connected');
    
    ws.on('message', function incoming(message) {
        console.log('Received:', message);
        // Broadcast to all clients
        wss.clients.forEach(function each(client) {
            if (client.readyState === WebSocket.OPEN) {
                client.send(message);
            }
        });
    });
    
    ws.send('Welcome to the WebSocket server!');
});
```

### 38. Explain the concept of tree shaking in JavaScript.
**Answer:** Tree shaking is a dead code elimination technique that removes unused code from the final bundle.

**How it works:**
```javascript
// math.js
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

export function multiply(a, b) {
    return a * b;
}

// app.js - Only imports 'add'
import { add } from './math.js';
console.log(add(2, 3));

// After tree shaking, only 'add' function is included in the bundle

// Webpack configuration for tree shaking
// webpack.config.js
module.exports = {
    mode: 'production', // Enables tree shaking
    optimization: {
        usedExports: true,
        minimize: true
    }
};

// package.json requirements
{
    "sideEffects": false, // or specify files with side effects
    "module": "esm/index.js" // ES module entry point
}

// Common patterns that prevent tree shaking:
// 1. Dynamic imports
// 2. eval() statements
// 3. Module-level side effects
// 4. Re-exports
```

### 39. What is code splitting? Why is it important?
**Answer:** Code splitting splits your code into various bundles that can be loaded on demand.

**Importance:**
1. Reduces initial load time
2. Improves performance
3. Better caching
4. On-demand loading

**Implementation:**
```javascript
// Dynamic import (lazy loading)
const loadModule = async () => {
    const module = await import('./heavyModule.js');
    module.heavyFunction();
};

// React.lazy (React example)
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

function MyComponent() {
    return (
        <React.Suspense fallback={<div>Loading...</div>}>
            <HeavyComponent />
        </React.Suspense>
    );
}

// Webpack dynamic imports
import(/* webpackChunkName: "chart" */ './charts')
    .then(chartModule => {
        chartModule.renderChart();
    });

// Route-based code splitting (React Router)
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

function App() {
    return (
        <Router>
            <Suspense fallback={<div>Loading...</div>}>
                <Routes>
                    <Route path="/" element={<Home />} />
                    <Route path="/about" element={<About />} />
                </Routes>
            </Suspense>
        </Router>
    );
}
```

### 40. Explain the concept of immutability in JavaScript.
**Answer:** Immutability means data cannot be changed after creation.

**Benefits:**
1. Predictable state
2. Easier debugging
3. Better performance with shallow equality checks
4. Thread safety

**Implementing Immutability:**
```javascript
// 1. Using const (only prevents reassignment, not mutation)
const arr = [1, 2, 3];
arr.push(4); // Mutates array (allowed)
// arr = [5, 6, 7]; // Error (reassignment not allowed)

// 2. Object.freeze (shallow immutability)
const obj = Object.freeze({ a: 1, b: { c: 2 } });
obj.a = 2; // Fails silently in non-strict mode
obj.b.c = 3; // Still works (nested objects not frozen)

// 3. Deep freeze
function deepFreeze(obj) {
    Object.keys(obj).forEach(key => {
        if (typeof obj[key] === 'object' && obj[key] !== null) {
            deepFreeze(obj[key]);
        }
    });
    return Object.freeze(obj);
}

// 4. Immutable operations (create new objects/arrays)
const numbers = [1, 2, 3];

// Bad (mutable)
numbers.push(4); // Mutates original

// Good (immutable)
const newNumbers = [...numbers, 4]; // Creates new array
const newNumbers2 = numbers.concat(4); // Creates new array

// Object spread for immutability
const user = { name: 'John', age: 30 };
const updatedUser = { ...user, age: 31 }; // New object

// Immutable.js library
import { Map, List } from 'immutable';

const map1 = Map({ a: 1, b: 2 });
const map2 = map1.set('b', 3); // Returns new Map

const list1 = List([1, 2, 3]);
const list2 = list1.push(4); // Returns new List
```

### 41. What are JavaScript iterators and iterables?
**Answer:** Iterators are objects that implement the iterator protocol (next() method). Iterables are objects that implement the @@iterator method.

**Examples:**
```javascript
// Custom iterator
const range = {
    from: 1,
    to: 5,
    
    [Symbol.iterator]() {
        this.current = this.from;
        return this;
    },
    
    next() {
        if (this.current <= this.to) {
            return { done: false, value: this.current++ };
        } else {
            return { done: true };
        }
    }
};

// Using the iterator
for (let num of range) {
    console.log(num); // 1, 2, 3, 4, 5
}

// Spread operator works with iterables
console.log([...range]); // [1, 2, 3, 4, 5]

// Generator as iterator
function* generateSequence() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = generateSequence();
console.log(generator.next()); // { value: 1, done: false }
console.log(generator.next()); // { value: 2, done: false }
console.log(generator.next()); // { value: 3, done: false }
console.log(generator.next()); // { value: undefined, done: true }

// Built-in iterables
const arr = [1, 2, 3];
const arrIterator = arr[Symbol.iterator]();

const str = 'hello';
const strIterator = str[Symbol.iterator]();

const map = new Map([['a', 1], ['b', 2]]);
const mapIterator = map[Symbol.iterator]();

// Using for...of (works with iterables)
for (const char of 'hello') {
    console.log(char);
}

for (const [key, value] of map) {
    console.log(key, value);
}
```

### 42. Explain the Proxy and Reflect objects in JavaScript.
**Answer:** Proxy wraps an object and intercepts operations. Reflect provides methods for interceptable operations.

**Proxy Example:**
```javascript
const target = {
    name: 'John',
    age: 30
};

const handler = {
    get(obj, prop) {
        console.log(`Getting property: ${prop}`);
        return prop in obj ? obj[prop] : 'Property not found';
    },
    
    set(obj, prop, value) {
        console.log(`Setting property: ${prop} = ${value}`);
        if (prop === 'age' && typeof value !== 'number') {
            throw new TypeError('Age must be a number');
        }
        obj[prop] = value;
        return true;
    },
    
    deleteProperty(obj, prop) {
        console.log(`Deleting property: ${prop}`);
        if (prop in obj) {
            delete obj[prop];
            return true;
        }
        return false;
    },
    
    has(obj, prop) {
        console.log(`Checking property: ${prop}`);
        return prop in obj;
    }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // Logs and returns 'John'
proxy.age = 31;         // Logs and sets age
console.log('age' in proxy); // Logs and returns true

// Reflect usage
const obj = { a: 1 };

// Instead of obj[prop]
console.log(Reflect.get(obj, 'a')); // 1

// Instead of obj[prop] = value
Reflect.set(obj, 'b', 2); // true

// Instead of prop in obj
console.log(Reflect.has(obj, 'a')); // true

// Instead of delete obj[prop]
Reflect.deleteProperty(obj, 'a'); // true

// Practical use cases:
// 1. Validation
const validator = {
    set(target, prop, value) {
        if (prop === 'email' && !value.includes('@')) {
            throw new Error('Invalid email');
        }
        return Reflect.set(target, prop, value);
    }
};

// 2. Logging
const logger = {
    get(target, prop) {
        console.log(`Accessed: ${prop}`);
        return Reflect.get(target, prop);
    }
};

// 3. Default values
const withDefaults = {
    get(target, prop) {
        return prop in target ? target[prop] : 'default';
    }
};
```

### 43. What are tagged template literals?
**Answer:** Tagged templates are function calls that receive template literal strings and expressions.

**Examples:**
```javascript
// Basic tagged template
function tag(strings, ...values) {
    console.log(strings); // Array of string literals
    console.log(values);  // Array of expressions
    return 'Processed string';
}

const name = 'John';
const age = 30;
const result = tag`Hello ${name}, you are ${age} years old.`;

// Practical examples:

// 1. SQL query sanitization
function sql(strings, ...values) {
    let query = '';
    strings.forEach((string, i) => {
        query += string;
        if (i < values.length) {
            query += `$${i + 1}`; // Parameterized query
        }
    });
    return { query, params: values };
}

const userId = 123;
const userQuery = sql`SELECT * FROM users WHERE id = ${userId}`;

// 2. HTML escaping
function html(strings, ...values) {
    return strings.reduce((result, str, i) => {
        const value = values[i] ? 
            String(values[i])
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;') : '';
        return result + str + value;
    }, '');
}

const userInput = '<script>alert("XSS")</script>';
const safeHTML = html`<div>${userInput}</div>`;

// 3. Internationalization
function i18n(strings, ...values) {
    const translations = {
        'Hello {name}, you have {count} messages.': 
            (name, count) => `Hola ${name}, tienes ${count} mensajes.`
    };
    
    const key = strings.join('{placeholder}');
    const translation = translations[key];
    return translation ? translation(...values) : strings.join('');
}

// 4. Styled components (like in React)
function styled(strings, ...values) {
    return function(props) {
        let style = '';
        strings.forEach((string, i) => {
            style += string;
            if (values[i]) {
                style += typeof values[i] === 'function' ? 
                    values[i](props) : values[i];
            }
        });
        return style;
    };
}

const Button = styled`
    background: ${props => props.primary ? 'blue' : 'gray'};
    color: white;
    padding: 10px 20px;
`;
```

### 44. Explain the concept of pure functions.
**Answer:** Pure functions always return the same output for the same input and have no side effects.

**Characteristics:**
1. No side effects
2. Same input → same output
3. No dependency on external state

**Examples:**
```javascript
// Pure functions
function add(a, b) {
    return a + b;
}

function capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}

function getFullName(user) {
    return `${user.firstName} ${user.lastName}`;
}

// Impure functions
let counter = 0;
function increment() {
    counter++; // Side effect: modifies external variable
    return counter;
}

function getRandom() {
    return Math.random(); // Different output for same input
}

function getUserFromDB(id) {
    // Side effect: depends on external state (database)
    return database.query(`SELECT * FROM users WHERE id = ${id}`);
}

// Benefits of pure functions:
// 1. Predictable
// 2. Easy to test
// 3. Cacheable (memoization)
// 4. Parallelizable

// Making impure functions pure:
// Impure
function formatDate(date) {
    return date.toLocaleDateString('en-US'); // Depends on locale
}

// Pure
function formatDatePure(date, locale = 'en-US') {
    return date.toLocaleDateString(locale); // Locale is now an input
}

// Another example:
// Impure (modifies input)
function addToCart(cart, item) {
    cart.push(item); // Modifies original cart
    return cart;
}

// Pure (creates new array)
function addToCartPure(cart, item) {
    return [...cart, item]; // Returns new array
}
```

### 45. What are higher-order functions? Provide examples.
**Answer:** Higher-order functions are functions that take other functions as arguments or return functions.

**Examples:**
```javascript
// 1. Functions that take functions as arguments
function calculate(a, b, operation) {
    return operation(a, b);
}

const result = calculate(5, 3, (x, y) => x + y); // 8

// Array methods (higher-order functions)
const numbers = [1, 2, 3, 4, 5];

// map - transforms each element
const doubled = numbers.map(n => n * 2); // [2, 4, 6, 8, 10]

// filter - selects elements
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]

// reduce - accumulates values
const sum = numbers.reduce((total, n) => total + n, 0); // 15

// forEach - executes for each element
numbers.forEach(n => console.log(n));

// 2. Functions that return functions
function multiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// 3. Function composition
function compose(f, g) {
    return function(x) {
        return f(g(x));
    };
}

const add1 = x => x + 1;
const multiply2 = x => x * 2;

const addThenMultiply = compose(multiply2, add1);
console.log(addThenMultiply(5)); // 12 (5 + 1 = 6, then 6 * 2 = 12)

// 4. Custom higher-order function
function withLogging(fn) {
    return function(...args) {
        console.log(`Calling ${fn.name} with arguments:`, args);
        const result = fn(...args);
        console.log(`Result:`, result);
        return result;
    };
}

const loggedAdd = withLogging((a, b) => a + b);
loggedAdd(2, 3); // Logs and returns 5

// 5. Partial application
function partial(fn, ...fixedArgs) {
    return function(...remainingArgs) {
        return fn(...fixedArgs, ...remainingArgs);
    };
}

function greet(greeting, name) {
    return `${greeting}, ${name}!`;
}

const sayHello = partial(greet, 'Hello');
console.log(sayHello('John')); // "Hello, John!"
```

### 46. Explain the concept of function composition.
**Answer:** Function composition is combining two or more functions to produce a new function.

**Examples:**
```javascript
// Simple composition
const compose = (f, g) => x => f(g(x));

const add1 = x => x + 1;
const multiply2 = x => x * 2;

const addThenMultiply = compose(multiply2, add1);
console.log(addThenMultiply(5)); // 12

// Multiple functions composition
const composeMultiple = (...fns) => 
    x => fns.reduceRight((acc, fn) => fn(acc), x);

const add5 = x => x + 5;
const square = x => x * x;
const subtract3 = x => x - 3;

const complexOperation = composeMultiple(
    subtract3,
    square,
    add5
);

console.log(complexOperation(2)); // 46 (2+5=7, 7*7=49, 49-3=46)

// Pipe (left-to-right composition)
const pipe = (...fns) => 
    x => fns.reduce((acc, fn) => fn(acc), x);

const pipedOperation = pipe(
    add5,
    square,
    subtract3
);

console.log(pipedOperation(2)); // Same result: 46

// Practical example: Data processing pipeline
const users = [
    { name: 'John', age: 25, active: true },
    { name: 'Jane', age: 30, active: false },
    { name: 'Bob', age: 35, active: true }
];

// Helper functions
const filterActive = users => users.filter(u => u.active);
const sortByAge = users => users.sort((a, b) => a.age - b.age);
const getNames = users => users.map(u => u.name);

// Compose the pipeline
const getActiveUserNames = pipe(
    filterActive,
    sortByAge,
    getNames
);

console.log(getActiveUserNames(users)); // ['John', 'Bob']

// Another example: String transformation
const trim = str => str.trim();
const toLowerCase = str => str.toLowerCase();
const removeSpaces = str => str.replace(/\s+/g, '');
const addPrefix = prefix => str => `${prefix}${str}`;

const normalizeUsername = pipe(
    trim,
    toLowerCase,
    removeSpaces,
    addPrefix('@')
);

console.log(normalizeUsername('  John Doe  ')); // "@johndoe"

// Composition with error handling
const safeCompose = (...fns) => x => {
    try {
        return fns.reduceRight((acc, fn) => fn(acc), x);
    } catch (error) {
        console.error('Composition error:', error);
        return null;
    }
};
```

### 47. What is the difference between Object.create() and new keyword?
**Answer:** Both create objects but with different inheritance mechanisms.

**Comparison:**
```javascript
// Using constructor function with 'new'
function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

const john = new Person('John');
console.log(john.greet()); // "Hello, I'm John"

// Using Object.create()
const personPrototype = {
    greet() {
        return `Hello, I'm ${this.name}`;
    }
};

const jane = Object.create(personPrototype);
jane.name = 'Jane';
console.log(jane.greet()); // "Hello, I'm Jane"

// Key differences:
const obj1 = new Person('Alice');
const obj2 = Object.create(Person.prototype);
Person.call(obj2, 'Bob');

console.log(obj1 instanceof Person); // true
console.log(obj2 instanceof Person); // true

// Object.create() can create objects without constructors
const animal = {
    makeSound() {
        return this.sound;
    }
};

const dog = Object.create(animal);
dog.sound = 'Woof!';
console.log(dog.makeSound()); // "Woof!"

// Setting properties during creation
const cat = Object.create(animal, {
    sound: {
        value: 'Meow!',
        writable: true,
        enumerable: true,
        configurable: true
    },
    color: {
        value: 'black',
        writable: false
    }
});

// Creating null prototype object
const dict = Object.create(null); // No inherited properties
dict.name = 'Dictionary';
console.log(dict.toString); // undefined
console.log('toString' in dict); // false
```

### 48. Explain the concept of memoization with recursive functions.
**Answer:** Memoization with recursion caches results of expensive recursive calls.

**Examples:**
```javascript
// Fibonacci without memoization (inefficient)
function fib(n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}

// Fibonacci with memoization
function memoizedFib() {
    const cache = {};
    
    function fib(n) {
        if (n in cache) {
            return cache[n];
        }
        
        if (n <= 1) {
            return n;
        }
        
        cache[n] = fib(n - 1) + fib(n - 2);
        return cache[n];
    }
    
    return fib;
}

const fibMemo = memoizedFib();
console.log(fibMemo(50)); // Fast

// Generic memoization for recursive functions
function memoize(fn) {
    const cache = new Map();
    
    const memoized = function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
    
    // Copy function name if possible
    memoized.name = fn.name;
    return memoized;
}

// Factorial with memoization
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

const memoizedFactorial = memoize(factorial);
console.log(memoizedFactorial(5)); // 120
console.log(memoizedFactorial(5)); // Returns cached

// For recursive functions, need special handling
function memoizeRecursive(fn) {
    const cache = new Map();
    
    const memoized = function recur(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            return cache.get(key);
        }
        
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
    
    return memoized;
}

// Deep equality check with memoization
function deepEqual(a, b) {
    if (a === b) return true;
    if (typeof a !== 'object' || typeof b !== 'object' || 
        a === null || b === null) {
        return false;
    }
    
    const keysA = Object.keys(a);
    const keysB = Object.keys(b);
    
    if (keysA.length !== keysB.length) return false;
    
    for (const key of keysA) {
        if (!keysB.includes(key) || !deepEqual(a[key], b[key])) {
            return false;
        }
    }
    
    return true;
}

const memoizedDeepEqual = memoize(deepEqual);
```

### 49. What are JavaScript Symbols? When would you use them?
**Answer:** Symbols are unique and immutable primitive values, often used as object property keys.

**Characteristics and Usage:**
```javascript
// Creating symbols
const sym1 = Symbol();
const sym2 = Symbol('description'); // Optional description
const sym3 = Symbol('description');

console.log(sym1 === sym2); // false (each symbol is unique)
console.log(sym2 === sym3); // false (even with same description)

// Using symbols as object keys
const obj = {
    [Symbol('id')]: 123,
    name: 'John'
};

console.log(Object.keys(obj)); // ['name'] (symbols not included)
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(id)]

// Well-known symbols
const iterable = {
    [Symbol.iterator]: function*() {
        yield 1;
        yield 2;
        yield 3;
    }
};

console.log([...iterable]); // [1, 2, 3]

// Symbol.for() and Symbol.keyFor()
const globalSym1 = Symbol.for('app.id');
const globalSym2 = Symbol.for('app.id');
console.log(globalSym1 === globalSym2); // true (global registry)

console.log(Symbol.keyFor(globalSym1)); // 'app.id'

// Use cases:

// 1. Private properties (convention only)
const _counter = Symbol('counter');
const _increment = Symbol('increment');

class Counter {
    constructor() {
        this[_counter] = 0;
    }
    
    [_increment]() {
        this[_counter]++;
    }
    
    get count() {
        return this[_counter];
    }
}

// 2. Preventing property name collisions
const META = {
    VERSION: Symbol('version'),
    AUTHOR: Symbol('author')
};

const library = {
    name: 'MyLib',
    [META.VERSION]: '1.0.0',
    [META.AUTHOR]: 'John Doe'
};

// 3. Defining protocols
const RENDERABLE = Symbol.for('renderable');

class Component {
    [RENDERABLE]() {
        return '<div>Component</div>';
    }
}

function render(component) {
    if (RENDERABLE in component) {
        return component[RENDERABLE]();
    }
    throw new Error('Not renderable');
}

// 4. Enum-like behavior
const COLOR = {
    RED: Symbol('red'),
    GREEN: Symbol('green'),
    BLUE: Symbol('blue')
};

function getColorName(color) {
    switch(color) {
        case COLOR.RED: return 'Red';
        case COLOR.GREEN: return 'Green';
        case COLOR.BLUE: return 'Blue';
    }
}
```

### 50. Explain the concept of Temporal Dead Zone (TDZ).
**Answer:** TDZ is the period between entering scope and variable declaration where the variable cannot be accessed.

**Examples:**
```javascript
// let and const have TDZ
console.log(myVar); // undefined (hoisted)
var myVar = 1;

// console.log(myLet); // ReferenceError (TDZ)
let myLet = 2;

// console.log(myConst); // ReferenceError (TDZ)
const myConst = 3;

// Function scope with TDZ
function test() {
    console.log(a); // undefined
    console.log(b); // ReferenceError (TDZ)
    
    var a = 1;
    let b = 2;
}

// Block scope with TDZ
{
    // console.log(blockVar); // ReferenceError (TDZ)
    let blockVar = 'inside';
}
// console.log(blockVar); // ReferenceError (out of scope)

// TDZ in loops
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100); // 0, 1, 2
}

// Without TDZ (using var)
for (var j = 0; j < 3; j++) {
    setTimeout(() => console.log(j), 100); // 3, 3, 3
}

// TDZ with temporal variables
function tricky() {
    const func = () => {
        console.log(value); // ReferenceError (TDZ)
    };
    
    let value = 'hello';
    func(); // Works if called after declaration
}

// TDZ in class declarations
{
    // const obj = new MyClass(); // ReferenceError (TDZ)
    
    class MyClass {
        constructor() {
            console.log('Created');
        }
    }
    
    const obj = new MyClass(); // Works
}

// Why TDZ exists:
// 1. Catch programming errors (using before declaration)
// 2. Make const work properly
// 3. Improve code readability

// Best practices to avoid TDZ issues:
// 1. Declare variables at the top of their scope
// 2. Initialize variables when declaring
// 3. Use const by default, let when needed, avoid var
```

---
