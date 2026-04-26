# 03. Vue 3 Core Concepts

## 3.1 Composition API
The Composition API is Vue 3’s way of organizing component logic by feature rather than by option type.

### Why it exists
In large components, Options API logic can get split across `data`, `methods`, `computed`, and lifecycle hooks. Composition API lets related logic stay together.

### `setup()`
`setup()` is where Composition API logic lives.

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
const increment = () => count.value++
</script>
```

### Benefits
- better logic reuse
- better TypeScript support
- clearer organization in complex components
- easier extraction into composables

## 3.2 `ref()`
Used for reactive primitive values and also works with objects.

```js
const count = ref(0)
count.value++
```

### Important detail
In JavaScript, you access it with `.value`.
In templates, Vue unwraps it automatically.

## 3.3 `reactive()`
Used to create a reactive object.

```js
const user = reactive({
  name: 'Bilal',
  age: 25
})
```

### When to prefer it
- grouped object state
- forms
- nested structures

### Caution
Destructuring reactive objects can break reactivity unless handled carefully.

## 3.4 `ref` vs `reactive`

### Use `ref` when
- working with primitives
- needing a single value
- returning values from composables
- wanting predictable access pattern

### Use `reactive` when
- working with grouped object state
- you want object-style property access

### Interview-safe answer
Use `ref` for primitives and often by default in composables. Use `reactive` for structured object state. Both create reactive data, but `ref` uses `.value` in JavaScript while `reactive` wraps an object directly.

## 3.5 `computed()`
Computed properties derive new values from reactive state.

```js
const firstName = ref('Bilal')
const lastName = ref('Haider')
const fullName = computed(() => `${firstName.value} ${lastName.value}`)
```

### Why use computed
- cached based on dependencies
- ideal for derived state
- cleaner than repeating logic in templates

## 3.6 `watch()`
Used to run side effects when reactive data changes.

```js
watch(searchTerm, (newValue) => {
  fetchResults(newValue)
})
```

### Use cases
- API calls on state changes
- syncing with localStorage
- reacting to route params

### Caution
Do not use `watch` when a `computed` property is enough.

## 3.7 `watchEffect()`
Automatically tracks reactive dependencies used inside its callback.

```js
watchEffect(() => {
  console.log(count.value)
})
```

### Difference from `watch`
- `watch` is explicit about the source
- `watchEffect` is automatic and immediate

## 3.8 Lifecycle Hooks

### Common hooks
- `onBeforeMount`
- `onMounted`
- `onBeforeUpdate`
- `onUpdated`
- `onBeforeUnmount`
- `onUnmounted`

### Practical uses
- `onMounted`: fetch browser-only data, initialize libraries, DOM access
- `onUnmounted`: cleanup listeners, timers, subscriptions

### Interview point
In SSR apps like Nuxt, browser-only code usually belongs in `onMounted` or client-only logic.

## 3.9 Template Refs
Used to access DOM nodes or child component instances.

```vue
<script setup>
import { ref, onMounted } from 'vue'

const inputRef = ref(null)

onMounted(() => {
  inputRef.value.focus()
})
</script>

<template>
  <input ref="inputRef" />
</template>
```

### Good use cases
- focus management
- integrating third-party DOM libraries
- measurements

### Bad use cases
- replacing normal reactive data flow
- manipulating DOM unnecessarily

## 3.10 Composable Thinking
Composition API encourages extracting reusable logic into composables.

Example:
- `useFetchUser()`
- `useAuth()`
- `useDebounce()`

This keeps components lean and promotes reuse.

## Common mistakes
- Forgetting `.value` in JavaScript
- Using `watch` for derived values instead of `computed`
- Overusing reactive objects everywhere
- Accessing DOM before mount
- Writing huge `setup()` blocks without composables

## Interview quick answers

### What is Composition API?
A Vue 3 API style that organizes logic by feature using functions like `ref`, `reactive`, `computed`, and `watch`.

### Difference between `computed` and `watch`?
`computed` is for derived state and is cached. `watch` is for side effects when data changes.

### Difference between `watch` and `watchEffect`?
`watch` explicitly tracks a source. `watchEffect` automatically tracks dependencies used in its callback.

### When do you use `onMounted`?
When you need DOM access, browser APIs, or to initialize code that should run after the component is mounted.
