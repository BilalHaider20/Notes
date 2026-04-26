# 01. JavaScript Foundations

## 1.1 Language Basics

### Variables: `let`, `const`, `var`
- `var` is function-scoped and can be redeclared. It is generally avoided in modern JavaScript because it can cause hoisting-related bugs and confusing scope behavior.
- `let` is block-scoped and can be reassigned.
- `const` is block-scoped and cannot be reassigned, but objects and arrays declared with `const` can still have their contents changed.

**Interview angle:**
- Explain why `const` is preferred by default.
- Explain hoisting and temporal dead zone for `let` and `const`.

### Data types
Primitive types:
- string
- number
- bigint
- boolean
- undefined
- symbol
- null

Reference types:
- object
- array
- function
- date
- map
- set

**Key point:** primitives are copied by value, objects are copied by reference.

### Type coercion
JavaScript sometimes converts values automatically.
Examples:
- `'5' + 1` becomes `'51'`
- `'5' - 1` becomes `4`
- `Boolean('')` is `false`
- `Boolean('hello')` is `true`

**Interview angle:**
- Prefer explicit conversion when possible.
- Understand `==` vs `===`. Most production code uses `===`.

### Truthy and falsy values
Falsy values:
- `false`
- `0`
- `''`
- `null`
- `undefined`
- `NaN`

Everything else is generally truthy.

### Destructuring
Used to extract values from arrays and objects.

```js
const user = { name: 'Ali', age: 25 }
const { name, age } = user
```

### Spread and rest
- Spread expands values.
- Rest collects remaining values.

```js
const nums = [1, 2, 3]
const copy = [...nums]

function sum(...args) {
  return args.reduce((a, b) => a + b, 0)
}
```

## 1.2 Functions

### Function declarations vs expressions
```js
function greet() {}
const greetUser = function () {}
const greetArrow = () => {}
```

### Arrow functions
- Shorter syntax
- Do not bind their own `this`
- Good for callbacks
- Not ideal for object methods when `this` is needed

### Higher-order functions
A function that takes another function as argument or returns a function.
Examples:
- `map`
- `filter`
- `reduce`
- custom utility functions

### Closures
A closure happens when a function remembers variables from its outer scope even after the outer function has finished running.

```js
function counter() {
  let count = 0
  return function () {
    count++
    return count
  }
}
```

**Interview angle:**
- Closures are heavily used in composables, event handlers, and encapsulation patterns.

### `this` binding
`this` depends on how a function is called, not where it is defined, except arrow functions which inherit `this` lexically.

### `call`, `apply`, `bind`
- `call` invokes immediately with arguments one by one
- `apply` invokes immediately with arguments as array
- `bind` returns a new function

## 1.3 Arrays and Objects

### Important array methods
- `map`: transform each item
- `filter`: keep matching items
- `reduce`: combine into one result
- `find`: get first matching item
- `some`: check if any item matches
- `every`: check if all items match

### Mutating vs non-mutating methods
Mutating:
- `push`
- `pop`
- `splice`
- `sort`
- `reverse`

Non-mutating:
- `map`
- `filter`
- `slice`
- `concat`

**Interview angle:**
In Vue and state management, mutability matters because careless mutation can make code harder to reason about.

### Shallow vs deep copy
```js
const original = { user: { name: 'Bilal' } }
const shallow = { ...original }
```
This only copies the top level. Nested objects still share the same reference.

## 1.4 Asynchronous JavaScript

### Event loop basics
JavaScript runs on a single call stack but can manage async operations through the event loop.

Important parts:
- call stack
- Web APIs / runtime APIs
- callback queue
- microtask queue

### Promises
A Promise represents a future result.
States:
- pending
- fulfilled
- rejected

```js
fetch('/api/user')
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err))
```

### `async/await`
Makes async code easier to read.

```js
async function loadUser() {
  try {
    const res = await fetch('/api/user')
    const data = await res.json()
    return data
  } catch (error) {
    console.error(error)
  }
}
```

### Promise helpers
- `Promise.all` for parallel success-dependent tasks
- `Promise.allSettled` when you want all results regardless of failure
- `Promise.race` for first settled result

## 1.5 DOM and Browser Concepts

### Event propagation
- Capturing phase
- Target phase
- Bubbling phase

### Debouncing and throttling
- Debounce: run after user stops triggering event
- Throttle: run at most once in a time window

Common usage:
- search input
- scroll listeners
- resize listeners

### Storage
- `localStorage`: persists across sessions
- `sessionStorage`: cleared when session/tab ends

## 1.6 Modern JS for Framework Work

### ES modules
```js
export function add(a, b) {
  return a + b
}

import { add } from './math.js'
```

### Optional chaining
```js
user?.profile?.name
```
Prevents runtime errors when a property is missing.

### Nullish coalescing
```js
const username = value ?? 'Guest'
```
Only falls back on `null` or `undefined`, not on all falsy values.

### Dynamic imports
Useful for lazy loading.

```js
const module = await import('./utils.js')
```

## Common mistakes
- Using `var` in modern apps
- Confusing `==` with `===`
- Forgetting that objects are copied by reference
- Misusing `this`
- Not handling async errors
- Blocking UI with expensive synchronous work

## Interview quick answers

### What is closure?
A function retaining access to variables from its outer scope after the outer function has executed.

### Difference between `let`, `const`, and `var`?
`var` is function-scoped and hoisted differently. `let` and `const` are block-scoped. `const` cannot be reassigned.

### Difference between `==` and `===`?
`==` allows type coercion. `===` checks value and type strictly.

### What is the event loop?
A mechanism that lets JavaScript handle async operations by coordinating the call stack, queues, and runtime APIs.
