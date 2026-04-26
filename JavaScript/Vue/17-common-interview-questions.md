# 17. Common Interview Questions

## Vue.js Questions

### 1. What is the difference between `ref` and `reactive`?
- `ref` is usually used for single values, especially primitives.
- `reactive` is used for objects.
- `ref` uses `.value` in JavaScript.
- In templates, refs are auto-unwrapped.

### 2. What is the difference between `computed` and `watch`?
- `computed` is for derived state.
- `watch` is for side effects.
- `computed` is cached based on dependencies.
- `watch` is useful for async actions or external effects.

### 3. Why is `key` important in `v-for`?
It helps Vue track each item’s identity and update the DOM correctly. Without stable keys, Vue may reuse elements incorrectly and cause bugs.

### 4. What is one-way data flow in Vue?
Data flows down through props and events flow up through emits. This makes state changes easier to trace.

### 5. What are slots in Vue?
Slots let parent components pass content into child components. They help create flexible reusable components.

### 6. What is a composable?
A composable is a reusable function that contains Vue reactive logic using Composition API.

### 7. What is the difference between `v-if` and `v-show`?
- `v-if` conditionally renders the element.
- `v-show` always renders it and just toggles CSS display.

### 8. What are lifecycle hooks used for?
They let you run code at different stages of a component’s life, such as mount, update, and unmount.

## Nuxt.js Questions

### 9. What is Nuxt and how is it different from Vue?
Vue is a UI framework. Nuxt is a higher-level framework built on Vue that provides SSR, file-based routing, layouts, middleware, and server endpoints.

### 10. What is SSR?
Server-side rendering means the HTML is rendered on the server before it reaches the browser.

### 11. What is hydration?
Hydration is the process where the client-side Vue app takes over the server-rendered HTML and makes it interactive.

### 12. What causes hydration mismatch?
Hydration mismatch happens when the HTML generated on the server differs from what the client expects to render.

Common causes:
- using `window` during SSR
- non-deterministic values like `Math.random()` or current time in render
- client-only code executed on server

### 13. Difference between `useFetch`, `useAsyncData`, and `$fetch`?
- `useFetch`: SSR-aware fetch composable with state helpers
- `useAsyncData`: more flexible SSR-aware async loader
- `$fetch`: low-level request helper

### 14. What is middleware in Nuxt?
Middleware runs before navigation to enforce rules like authentication or redirects.

### 15. What is runtime config?
Runtime config stores public and private environment-based values. Public values are exposed to the client, private ones stay on the server.

## JavaScript Questions

### 16. What is closure?
A closure is when a function remembers variables from its outer scope even after that outer function has finished executing.

### 17. What is the event loop?
The event loop coordinates execution of synchronous code, async callbacks, microtasks, and queued tasks.

### 18. Difference between `==` and `===`?
`==` does type coercion. `===` compares both type and value strictly.

### 19. What is the difference between `null` and `undefined`?
- `undefined` usually means a value has not been assigned.
- `null` is an intentional absence of value.

### 20. What is the difference between shallow copy and deep copy?
A shallow copy copies only the first level. Nested objects still reference the original. A deep copy duplicates nested structures too.

## Scenario Questions

### 21. How would you protect a Nuxt route?
Use route middleware to check authentication or authorization and redirect if access should be denied.

### 22. How would you handle API loading and error states?
Model them explicitly with reactive states or Nuxt fetch utilities and render loading, error, and success UI clearly.

### 23. How would you reuse logic across multiple Vue components?
Extract shared logic into a composable. If it is app-wide setup, maybe use a plugin or store depending on the problem.

### 24. How would you integrate a browser-only library into Nuxt?
Run it only on the client, usually in `onMounted`, a client-only plugin, or behind a client check.

### 25. How would you optimize a slow Vue page rendering a large list?
- use stable keys
- paginate or virtualize large lists
- avoid unnecessary watchers
- move expensive computations into cached computed properties
- lazy load non-critical components

## Strong answer strategy
When answering interview questions:
1. define the concept simply
2. explain when to use it
3. mention a tradeoff or common bug
4. give a short real-world example

That structure makes answers sound experienced instead of memorized.
