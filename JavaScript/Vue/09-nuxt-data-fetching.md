# 09. Nuxt Data Fetching

## 9.1 Core Data Fetching Tools

### `useFetch`
A Nuxt composable for fetching data with SSR awareness.

```js
const { data, pending, error, refresh } = await useFetch('/api/users')
```

### Why it is useful
- works with server-side rendering
- provides reactive `data`, `pending`, and `error`
- integrates well with Nuxt lifecycle

### `useAsyncData`
A more generic async data utility.

```js
const { data, pending, error } = await useAsyncData('users', () => $fetch('/api/users'))
```

### When to prefer it
- when you want explicit control
- when fetching from multiple sources
- when wrapping custom logic

### `$fetch`
A lightweight fetch utility often used in Nuxt server/client code.

```js
const users = await $fetch('/api/users')
```

### Difference summary
- `useFetch`: simple fetch with Nuxt-friendly state handling
- `useAsyncData`: general-purpose async loader for SSR-aware data
- `$fetch`: raw request utility, no auto state wrapper by itself

## 9.2 SSR-aware Fetching
In Nuxt, data fetching may run on the server during initial render and then hydrate on the client.

### Why this matters
- SEO and first paint improve
- server can access private resources safely in some cases
- bad code can cause duplicate requests or hydration mismatches

## 9.3 Client vs Server Execution

### On the server
You can safely access server runtime config and protected APIs.

### On the client
You can use browser APIs and user-triggered interactions.

### Interview point
A strong engineer knows where code runs and why it matters.

## 9.4 Avoiding Duplicate Requests
Common causes:
- fetching both in a page and child component without coordination
- using raw fetch incorrectly in lifecycle hooks
- not understanding hydration behavior

### Better approach
- centralize page-level fetches
- pass data down where possible
- use keys and Nuxt utilities correctly

## 9.5 Error and Pending States
Always handle loading and failure.

```vue
<template>
  <div v-if="pending">Loading...</div>
  <div v-else-if="error">Something went wrong</div>
  <ul v-else>
    <li v-for="user in data" :key="user.id">{{ user.name }}</li>
  </ul>
</template>
```

### Interview angle
Good frontend engineers design for unstable networks, not perfect demos.

## 9.6 Refreshing Data
Nuxt fetching composables often expose `refresh`.

Use cases:
- retry after error
- refetch after mutation
- manual refresh button

## 9.7 Route-driven Fetching
You often fetch based on route params.

Example:
- `/users/[id].vue`
- fetch user by `route.params.id`

### Caution
If params change on the same page instance, make sure the fetch reacts correctly.

## 9.8 Server API Routes
Nuxt can define backend endpoints inside the same project.

Example path:
- `server/api/users.ts`

### Benefits
- frontend and backend close together
- easy BFF pattern
- can hide private backend logic from client

## 9.9 Request Handling Basics
Server routes can access:
- params
- query
- request body
- headers
- cookies

### Practical uses
- auth endpoints
- data proxying
- server-side validation
- secure token exchange

## 9.10 Best Practices
- fetch at the correct level
- always model loading and error states
- avoid exposing secrets to client-side code
- understand where your request is executed
- use composables for reusable fetch logic

## Common mistakes
- Using browser-only APIs in server-side fetch logic
- Leaking private config to the client
- Fetching data in too many places
- Ignoring pending and error states
- Confusing `$fetch` with `useFetch`

## Interview quick answers

### Difference between `useFetch` and `$fetch`?
`useFetch` is a Nuxt composable with SSR-aware reactive state like `pending` and `error`. `$fetch` is the lower-level request utility.

### What is `useAsyncData` used for?
It is used to load async data in an SSR-aware way when you need more flexibility than `useFetch`.

### Why can Nuxt data fetching be tricky?
Because code may run on both server and client, which affects security, hydration, duplicate requests, and access to browser APIs.
