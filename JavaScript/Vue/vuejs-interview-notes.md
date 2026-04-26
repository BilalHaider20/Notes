# Vue.js Interview Preparation Notes

---

## 2. Vue.js Fundamentals

---

### 2.1 What Vue Is

#### Progressive Framework Concept

Vue is called "progressive" because you can adopt it incrementally. You can drop a `<script>` tag into an existing HTML page and add a little interactivity, or you can use it to build a full-scale SPA with routing, state management, and build tooling. You're never forced into an all-or-nothing adoption.

> **Interview angle:** "Progressive means you can start small and scale up. Vue doesn't require you to buy into the entire ecosystem from day one."

#### Declarative Rendering

Instead of writing imperative DOM manipulation (`document.querySelector`, `.innerHTML =`, etc.), you declare what the UI should look like based on state, and Vue figures out how to get there.

```html
<!-- Declarative: describe the outcome -->
<p>{{ message }}</p>
```

vs. imperative JavaScript:

```js
document.querySelector('p').textContent = message;
```

#### Component-Based Architecture

UIs are broken into self-contained, reusable components. Each component encapsulates its own template, logic, and styles. Components can be nested, composed, and reused across the app.

#### Reactive UI Model

Vue's reactivity system automatically tracks which components depend on which pieces of state. When state changes, only the components that depend on that state re-render. You never call `setState` or manually trigger updates — Vue handles it.

---

### 2.2 Vue Project Structure

#### Single File Components (SFCs)

An SFC is a `.vue` file that co-locates template, script, and styles in one file. This is the standard way to write Vue components in any serious project.

```vue
<template>
  <div>{{ title }}</div>
</template>

<script setup>
const title = 'Hello Vue';
</script>

<style scoped>
div { color: red; }
</style>
```

#### `<template>`, `<script>`, `<style>`


| Block                         | Purpose                                |
| ----------------------------- | -------------------------------------- |
| `<template>`                  | The HTML structure of the component    |
| `<script>` / `<script setup>` | Component logic, data, props, emits    |
| `<style>`                     | Component-level CSS, optionally scoped |


- `<style scoped>` applies styles only to the current component using a generated data attribute.
- `<style module>` enables CSS Modules.
- Multiple `<style>` blocks are allowed.

#### Local vs Global Components

- **Local:** Registered inside a specific component. Only usable within that component's template. Preferred approach for most components.
- **Global:** Registered with `app.component(...)`. Available everywhere but pollutes the global namespace and makes tree-shaking harder.

```js
// Global registration (main.js)
app.component('MyButton', MyButton);

// Local registration (inside a component)
import MyButton from './MyButton.vue';
// with <script setup>, import is enough — no explicit registration needed
```

#### File Organization Best Practices

- Group by feature/domain, not by file type.
- Keep components small and focused (single responsibility).
- Composables go in a `composables/` folder, stores in `stores/`, pages/views in `views/` or `pages/`.
- Prefix base/UI components (`BaseButton`, `AppModal`).

---

### 2.3 Vue Templates

#### Interpolation

`{{ expression }}` renders any JavaScript expression as text inside the DOM. It's HTML-escaped by default (XSS safe). For raw HTML use `v-html` (use carefully).

```html
<p>{{ user.name.toUpperCase() }}</p>
```

#### Attribute Binding — `v-bind`

Binds a dynamic value to an HTML attribute or component prop.

```html
<img v-bind:src="imageUrl" />
<!-- Shorthand -->
<img :src="imageUrl" />
<!-- Bind multiple attributes from an object -->
<div v-bind="attrs"></div>
```

#### Event Binding — `v-on`

Attaches a DOM event listener.

```html
<button v-on:click="handleClick">Click</button>
<!-- Shorthand -->
<button @click="handleClick">Click</button>
<!-- Inline handler -->
<button @click="count++">+</button>
```

#### Two-Way Binding — `v-model`

`v-model` is syntactic sugar. On a native input it expands to `:value` + `@input`. On a component it expands to `:modelValue` + `@update:modelValue`.

```html
<input v-model="name" />
<!-- Equivalent to: -->
<input :value="name" @input="name = $event.target.value" />
```

#### Conditional Rendering


| Directive   | Behavior                                     |
| ----------- | -------------------------------------------- |
| `v-if`      | Fully adds/removes element from DOM          |
| `v-else-if` | Chained condition                            |
| `v-else`    | Fallback                                     |
| `v-show`    | Toggles CSS `display` — element stays in DOM |


Use `v-if` when the condition is unlikely to change frequently (higher toggle cost). Use `v-show` when toggling often (higher initial render cost for `v-if`).

```html
<div v-if="role === 'admin'">Admin Panel</div>
<div v-else-if="role === 'editor'">Editor Panel</div>
<div v-else>Viewer</div>
```

#### List Rendering — `v-for`

```html
<ul>
  <li v-for="item in items" :key="item.id">{{ item.name }}</li>
</ul>
<!-- With index -->
<li v-for="(item, index) in items" :key="item.id">
```

`v-for` can iterate arrays, objects, or a number range (`v-for="n in 5"`).

#### `key` Usage and Importance

`key` is a special attribute that gives Vue a stable identity for each node in a list. Without it, Vue uses an "in-place patch" strategy that can produce incorrect results when items have internal state or when the list is reordered.

- Always use a **stable, unique** key (database IDs, not array index unless the list is static and never reordered).
- Index as key causes bugs when items are added/removed in the middle.

#### Event Modifiers and Key Modifiers

Modifiers let you handle common event patterns declaratively.

```html
<!-- .prevent calls event.preventDefault() -->
<form @submit.prevent="handleSubmit">

<!-- .stop calls event.stopPropagation() -->
<div @click.stop="doThis">

<!-- .once fires only once -->
<button @click.once="doOnce">

<!-- .self only triggers if target is the element itself -->
<div @click.self="handler">

<!-- Key modifiers -->
<input @keyup.enter="submit" />
<input @keyup.escape="cancel" />
```

---

### 2.4 Reactivity

#### What Reactivity Means in Vue

When you change a reactive variable, the UI automatically updates to reflect that change. You don't manually update the DOM.

#### Dependency Tracking

Vue 3 uses **Proxy**-based reactivity. When a reactive object's property is read during component rendering, Vue records that dependency. When the property is written to, Vue knows to re-run any effects (re-render, computed, watcher) that depend on it.

#### Reactive State Updates

Updates are batched and applied asynchronously (next microtask tick). This prevents multiple synchronous mutations from causing multiple renders.

```js
import { nextTick } from 'vue';
count.value++;
await nextTick(); // DOM is now updated
```

#### Why Vue Updates the DOM Efficiently

Vue uses a **Virtual DOM (VDOM)**. On state change, it creates a new VDOM tree, diffs it against the previous one, and only patches the real DOM where changes occurred. This minimizes expensive DOM operations.

#### Ref Unwrapping Behavior in Templates

In templates, `ref` values are automatically unwrapped — you don't need `.value`.

```js
const count = ref(0);
```

```html
<!-- In template: no .value needed -->
<p>{{ count }}</p>
```

But in JavaScript, you always need `.value`:

```js
count.value++;
```

**Caveat:** A `ref` nested inside a `reactive()` object is also automatically unwrapped when accessed, but only at the top level.

#### Common Reactivity Caveats

- Destructuring a `reactive()` object loses reactivity. Use `toRefs()` to preserve it.
- You cannot replace a `reactive()` object wholesale (the reference breaks).
- Arrays: direct index assignment `arr[0] = x` works with Vue 3's Proxy (unlike Vue 2). `arr.length = 0` also works.
- Adding new properties to `reactive()` objects is tracked automatically in Vue 3 (Proxy handles this; Vue 2 required `Vue.set`).

---

### 2.5 Component Communication

#### Props

Props pass data from parent to child. They are read-only in the child.

```vue
<!-- Parent -->
<UserCard :name="user.name" :age="user.age" />

<!-- Child -->
<script setup>
const props = defineProps({ name: String, age: Number });
</script>
```

#### Prop Validation

```js
defineProps({
  title: {
    type: String,
    required: true,
  },
  count: {
    type: Number,
    default: 0,
    validator: (val) => val >= 0,
  },
});
```

#### One-Way Data Flow

Data flows **down** from parent to child via props. The child must never mutate props directly. To "change" a prop, the child emits an event upward.

#### Custom Events with `emit`

```vue
<!-- Child -->
<script setup>
const emit = defineEmits(['update:modelValue', 'close']);

function handleClick() {
  emit('close');
}
</script>

<!-- Parent -->
<Modal @close="isOpen = false" />
```

#### Passing Functions vs Emitting Events

- **Emitting events** is the Vue-idiomatic pattern. It keeps parent/child decoupled and is easier to track in DevTools.
- **Passing a function as a prop** is sometimes seen in render functions or highly generic components, but should generally be avoided in favor of events for child-to-parent communication.

#### Slots

Slots let a parent inject content into a child component's template.

```vue
<!-- Child: MyCard.vue -->
<template>
  <div class="card">
    <slot /> <!-- default slot -->
  </div>
</template>

<!-- Parent -->
<MyCard>
  <p>This is injected content</p>
</MyCard>
```

#### Named Slots

```vue
<!-- Child -->
<template>
  <header><slot name="header" /></header>
  <main><slot /></main>
  <footer><slot name="footer" /></footer>
</template>

<!-- Parent -->
<MyLayout>
  <template #header>Title</template>
  <p>Main content</p>
  <template #footer>Footer text</template>
</MyLayout>
```

#### Scoped Slots

Allow the child to pass data back to the parent's slot content.

```vue
<!-- Child -->
<slot :item="currentItem" :index="i" />

<!-- Parent -->
<MyList v-slot="{ item, index }">
  <span>{{ index }}: {{ item.name }}</span>
</MyList>
```

#### Provide / Inject Basics

Used to avoid prop drilling across deeply nested components. An ancestor `provide`s a value; any descendant can `inject` it.

```js
// Ancestor
import { provide, ref } from 'vue';
const theme = ref('dark');
provide('theme', theme);

// Descendant
import { inject } from 'vue';
const theme = inject('theme');
```

Best practice: provide a read-only version to prevent descendant mutation, or provide a mutating function alongside the data.

---

## 3. Vue 3 Core Concepts

---

### 3.1 Composition API

#### Why Composition API Exists

The Options API (Vue 2 style) forced you to split related logic across `data`, `computed`, `methods`, `watch`, etc. In large components, logic for a single feature became fragmented. The Composition API lets you co-locate related logic and extract it into reusable composables.

#### `setup()`

The entry point of the Composition API. Runs before component creation, before `beforeCreate` and `created` hooks. `<script setup>` is the preferred syntactic sugar.

```vue
<script setup>
import { ref, computed } from 'vue';
const count = ref(0);
const double = computed(() => count.value * 2);
</script>
```

With explicit `setup()`:

```js
export default {
  setup(props, context) {
    // context: { attrs, slots, emit, expose }
    const count = ref(0);
    return { count }; // must return values for template
  }
}
```

#### `ref()`

Wraps a value (usually a primitive) in a reactive container.

```js
const count = ref(0);
count.value++; // mutate via .value in JS
```

#### `reactive()`

Makes an entire object reactive. Deeply reactive by default.

```js
const state = reactive({ count: 0, user: { name: 'Ali' } });
state.count++; // no .value needed
```

#### `computed()`

Creates a cached, derived reactive value. Only re-evaluates when dependencies change.

```js
const fullName = computed(() => `${firstName.value} ${lastName.value}`);
```

Writable computed:

```js
const name = computed({
  get: () => firstName.value + ' ' + lastName.value,
  set: (val) => { [firstName.value, lastName.value] = val.split(' '); }
});
```

#### `watch()`

Runs a side effect when a reactive source changes. Lazy by default (does not run on mount).

```js
watch(count, (newVal, oldVal) => {
  console.log(newVal, oldVal);
});
```

#### `watchEffect()`

Automatically tracks its reactive dependencies and re-runs whenever any of them change. Runs immediately on mount.

```js
watchEffect(() => {
  console.log(count.value); // runs now and on every count change
});
```

#### Lifecycle Hooks in Composition API

```js
import { onMounted, onUnmounted } from 'vue';

onMounted(() => { /* DOM is available */ });
onUnmounted(() => { /* cleanup */ });
```

#### Returning Values from `setup`

With `<script setup>`: everything declared at the top level is automatically exposed to the template.
With explicit `setup()`: you must return an object of everything the template needs.

---

### 3.2 `ref` vs `reactive`


|                     | `ref`                                 | `reactive`                        |
| ------------------- | ------------------------------------- | --------------------------------- |
| Use case            | Primitives and single values          | Objects and arrays                |
| Access pattern      | `.value` in JS, unwrapped in template | Direct property access            |
| Reassignment        | `count.value = 5` ✅                   | `state = {}` ❌ breaks reactivity  |
| Destructuring       | Safe (it's a single object)           | Loses reactivity — use `toRefs()` |
| Template unwrapping | Automatic                             | N/A                               |


**When to use `ref`:** For primitive values (number, string, boolean), or when you need to pass the reactive container itself (e.g., into a composable or template ref).

**When to use `reactive`:** For related state grouped in an object. But be careful about losing the reference.

**Nested object handling:** Both deeply track nested changes. `ref({ nested: { val: 1 } })` — the nested object is also reactive.

**Interop:** You can use `ref` for everything. Many teams standardize on `ref` only to avoid confusion. `toRef()` and `toRefs()` convert reactive object properties into refs.

---

### 3.3 Computed and Watchers

#### Computed Properties

- Cached: only re-evaluates when reactive dependencies change.
- Should be pure (no side effects).
- Perfect for derived state.

```js
const total = computed(() => items.value.reduce((sum, i) => sum + i.price, 0));
```

#### Watchers for Side Effects

Watchers are for things like API calls, DOM manipulation, logging, or synchronizing with external systems when state changes.

#### Immediate and Deep Watchers

```js
watch(user, (val) => {
  fetchProfile(val.id);
}, { immediate: true, deep: true });
```

- `immediate: true` — runs the callback right away on mount.
- `deep: true` — watches nested property changes in objects.

#### Watching Refs, Reactive Objects, and Getters

```js
// Single ref
watch(count, callback);

// Multiple sources
watch([count, name], ([newCount, newName]) => {});

// Reactive object property (must use getter)
watch(() => state.count, callback);

// Entire reactive object
watch(state, callback, { deep: true });
```

#### When NOT to Use Watchers

- Don't use a watcher to compute derived state — use `computed`.
- Don't use a watcher when `watchEffect` would be simpler.
- Don't use watchers to sync component state that could just use a computed setter or event.

---

### 3.4 Lifecycle Hooks


| Hook              | When it runs                      | Common use                                         |
| ----------------- | --------------------------------- | -------------------------------------------------- |
| `onBeforeMount`   | Before DOM insertion              | Rarely needed                                      |
| `onMounted`       | After DOM is inserted             | Fetch data, access DOM refs, init libraries        |
| `onBeforeUpdate`  | Before reactive changes re-render | Capture pre-update DOM state                       |
| `onUpdated`       | After re-render                   | Access updated DOM (prefer watchers when possible) |
| `onBeforeUnmount` | Before teardown                   | Cancel timers, abort requests                      |
| `onUnmounted`     | After teardown                    | Remove event listeners, cleanup subscriptions      |


**Real-world uses:**

- `onMounted` → call an API to load initial data, initialize a chart library, set focus.
- `onUnmounted` → clear `setInterval`, remove `window.addEventListener`, cancel fetch with AbortController.
- `onUpdated` → rarely needed; if you're using it a lot, consider watchers instead.

---

### 3.5 Template Refs

#### DOM Refs

Access a real DOM element directly.

```vue
<template>
  <input ref="inputEl" />
</template>

<script setup>
import { ref, onMounted } from 'vue';
const inputEl = ref(null);
onMounted(() => inputEl.value.focus());
</script>
```

#### Component Refs

Access a child component instance. With `<script setup>`, child must explicitly expose using `defineExpose`.

```vue
<!-- Child -->
<script setup>
defineExpose({ reset });
</script>

<!-- Parent -->
<ChildComponent ref="childRef" />
<script setup>
const childRef = ref(null);
childRef.value.reset(); // call child's exposed method
</script>
```

#### Access Timing

Template refs are only populated **after** the component is mounted. Accessing them before `onMounted` returns `null`.

#### Avoiding Overuse

Template refs bypass Vue's reactive data flow. Overusing them leads to imperative, hard-to-maintain code. Prefer props, emits, and reactive state whenever possible.

---

## 4. Advanced Vue

---

### 4.1 Composables

#### What Composables Are

Composables are plain JavaScript functions that use Vue's Composition API internally. They encapsulate and reuse stateful logic across components.

```js
// useCounter.js
import { ref } from 'vue';

export function useCounter(initialValue = 0) {
  const count = ref(initialValue);
  function increment() { count.value++; }
  function decrement() { count.value--; }
  return { count, increment, decrement };
}
```

#### Reusable Logic Extraction

Any logic that you find yourself duplicating across components (fetching data, form handling, event listeners, timers) is a candidate for a composable.

#### Naming Conventions

Always prefix with `use`: `useFetch`, `useAuth`, `useLocalStorage`, `useDebounce`.

#### Side-Effect Handling

Register side effects (event listeners, intervals) inside the composable and always clean them up in `onUnmounted`.

```js
export function useWindowResize() {
  const width = ref(window.innerWidth);
  function update() { width.value = window.innerWidth; }
  window.addEventListener('resize', update);
  onUnmounted(() => window.removeEventListener('resize', update));
  return { width };
}
```

#### Avoiding Shared State Bugs

Reactive state declared **inside** the composable function is created fresh per call. State declared **outside** the function is shared across all calls (useful for global singletons, but easy to cause bugs).

```js
// SHARED STATE — one instance for entire app
const globalCount = ref(0);
export function useGlobalCounter() {
  return { globalCount };
}

// INSTANCE STATE — fresh per component
export function useLocalCounter() {
  const count = ref(0); // new ref each time
  return { count };
}
```

#### Testing Composables

Test them as plain functions in a test environment. Use `@vue/test-utils` or plain Vitest. Wrap in a host component if lifecycle hooks are needed.

---

### 4.2 Custom Directives

#### When Directives Are Useful

When you need to perform low-level DOM manipulation that doesn't fit naturally into a component or composable. Good for behaviors like: auto-focus, tooltip initialization, click-outside detection, intersection observer attachment.

#### Directive Hooks

```js
app.directive('focus', {
  mounted(el, binding, vnode) {
    el.focus();
  },
  updated(el, binding) { /* ... */ },
  unmounted(el) { /* cleanup */ }
});
```

- `el` — the DOM element the directive is bound to.
- `binding` — contains `value`, `arg`, `modifiers`.

#### Common Use Cases

```js
// Click outside
app.directive('click-outside', {
  mounted(el, binding) {
    el._clickOutsideHandler = (e) => {
      if (!el.contains(e.target)) binding.value(e);
    };
    document.addEventListener('click', el._clickOutsideHandler);
  },
  unmounted(el) {
    document.removeEventListener('click', el._clickOutsideHandler);
  }
});
```

---

### 4.3 Dynamic and Async Components

#### `<component :is="">`

Renders a different component based on a dynamic value.

```vue
<component :is="currentTab === 'home' ? HomeTab : AboutTab" />
```

Can accept a component definition object or a registered component name string.

#### Lazy Loading Components

By default, all components in a bundle are loaded eagerly. Lazy loading defers loading until the component is actually needed.

#### `defineAsyncComponent`

```js
import { defineAsyncComponent } from 'vue';

const HeavyChart = defineAsyncComponent(() => import('./HeavyChart.vue'));
```

With loading/error states:

```js
const AsyncComp = defineAsyncComponent({
  loader: () => import('./Component.vue'),
  loadingComponent: Spinner,
  errorComponent: ErrorView,
  delay: 200,
  timeout: 3000,
});
```

#### Code Splitting Benefits

- Reduces initial bundle size.
- Improves time-to-interactive.
- Components are only downloaded when needed (e.g., modal only loads when opened).

---

### 4.4 Built-in Components

#### `<Transition>`

Applies enter/leave animations to a single element or component.

```vue
<Transition name="fade">
  <div v-if="show">Content</div>
</Transition>
```

CSS classes applied: `fade-enter-active`, `fade-enter-from`, `fade-leave-active`, `fade-leave-to`.

#### `<TransitionGroup>`

Applies animations to a list of elements (uses FLIP animation technique internally for smooth moves).

#### `<KeepAlive>`

Caches component instances instead of destroying them. Preserves state when switching between components.

```vue
<KeepAlive :include="['HomeTab']" :max="5">
  <component :is="currentTab" />
</KeepAlive>
```

`onActivated` and `onDeactivated` hooks fire instead of mount/unmount for kept-alive components.

#### `<Teleport>`

Renders a component's template at a different location in the DOM (e.g., body), regardless of where the component lives in the component tree. Perfect for modals, tooltips, and dropdowns.

```vue
<Teleport to="body">
  <Modal v-if="isOpen" />
</Teleport>
```

#### `<Suspense>`

Handles async components with loading fallback content declaratively.

```vue
<Suspense>
  <template #default><AsyncComponent /></template>
  <template #fallback><Spinner /></template>
</Suspense>
```

---

### 4.5 Performance Optimization

#### Avoid Unnecessary Re-renders

- Split large components into smaller ones so only relevant parts re-render.
- Avoid inline object/array props (create new reference every render).
- Use `v-once` for static content that never changes.
- Use `v-memo` to memoize subtrees.

#### Proper `key` Usage

Stable, unique keys prevent Vue from recycling DOM nodes incorrectly, especially in lists. Changing a key forces a component to fully remount (useful for resetting state).

#### Lazy Loading and Async Components

Load heavy components, routes, and libraries only when needed. This directly reduces the initial bundle size.

#### Bundle Optimization

- Use Vite or webpack with tree-shaking.
- Analyze bundle with `rollup-plugin-visualizer`.
- Import only what you use from large libraries.

#### Large List Strategies

- Use virtual scrolling (`vue-virtual-scroller` or `@tanstack/virtual`) for lists with thousands of items — only render what's visible.
- Paginate or infinitely scroll instead of rendering all at once.

#### Memoization Concepts

`computed` properties are memoized — they don't recompute unless dependencies change. Methods are NOT memoized — they run on every access.

#### Computed vs Methods Performance

```js
// Re-evaluates only when dependencies change
const sorted = computed(() => items.value.sort(...));

// Re-runs on every render
function getSorted() { return items.value.sort(...); }
```

Use `computed` in templates when the calculation is expensive or used in multiple places.

---

### 4.6 Error Handling and Debugging

#### Vue DevTools

Browser extension that lets you inspect component tree, props, reactive state, events, and performance. Invaluable for debugging.

#### Common Warning Messages

- `[Vue warn]: Missing required prop` — prop with `required: true` not passed.
- `[Vue warn]: Extraneous non-emits event listeners` — event emitted but not declared in `emits`.
- `[Vue warn]: Component emitted event but it is not declared` — add to `defineEmits`.

#### Tracking Reactive Bugs

Use `console.log(toRaw(state))` to see the plain value without the Proxy wrapper. Check if a value is truly reactive with `isRef()` / `isReactive()`.

#### Debugging Watchers and Props

Add logs in `watch` callbacks or use Vue DevTools' component inspector to see when props change. `watchEffect` can be debugged with `{ onTrack, onTrigger }` options.

#### Handling Component Errors

```js
// Global error handler
app.config.errorHandler = (err, instance, info) => {
  console.error(err, info);
};

// Per-component (Options API / Composition)
onErrorCaptured((err, instance, info) => {
  // return false to stop propagation
  return false;
});
```

---

## 5. Vue Router

---

### 5.1 Routing Basics

#### SPA Routing Concept

In a Single Page Application, navigation doesn't reload the whole page. Vue Router intercepts URL changes, matches them to route definitions, and renders the appropriate component in a `<RouterView>` outlet.

#### Defining Routes

```js
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
];
const router = createRouter({
  history: createWebHistory(),
  routes,
});
```

#### Route Params

```js
{ path: '/users/:id', component: UserProfile }

// In component:
import { useRoute } from 'vue-router';
const route = useRoute();
console.log(route.params.id);
```

#### Query Params

```js
// URL: /search?q=vue&page=2
const route = useRoute();
console.log(route.query.q);    // 'vue'
console.log(route.query.page); // '2'
```

#### Nested Routes

```js
{
  path: '/users/:id',
  component: UserLayout,
  children: [
    { path: 'profile', component: UserProfile },
    { path: 'posts', component: UserPosts },
  ]
}
// UserLayout.vue must contain a <RouterView />
```

#### Named Routes

```js
{ path: '/users/:id', name: 'UserProfile', component: UserProfile }

// Navigate:
router.push({ name: 'UserProfile', params: { id: 42 } });
```

#### Programmatic Navigation

```js
import { useRouter } from 'vue-router';
const router = useRouter();

router.push('/home');
router.push({ name: 'UserProfile', params: { id: 1 } });
router.replace('/login'); // replace instead of push (no history entry)
router.go(-1); // go back
```

---

### 5.2 Route Guards

#### Global Guards

```js
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isLoggedIn()) {
    next('/login');
  } else {
    next();
  }
});
```

`router.afterEach` runs after navigation and is useful for analytics.

#### Per-Route Guards

```js
{
  path: '/admin',
  component: Admin,
  beforeEnter: (to, from, next) => {
    if (!isAdmin()) next('/forbidden');
    else next();
  }
}
```

#### In-Component Guards

```js
// Composition API
import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router';

onBeforeRouteLeave((to, from, next) => {
  if (hasUnsavedChanges) {
    if (!confirm('Leave without saving?')) return next(false);
  }
  next();
});
```

#### Authentication Flow

1. User navigates to protected route.
2. `router.beforeEach` checks auth status (e.g., token in store or localStorage).
3. If not authenticated, redirect to `/login` with the intended route in query: `next({ path: '/login', query: { redirect: to.fullPath } })`.
4. After login, redirect to the original route.

#### Authorization Checks

Use route `meta` fields to declare requirements:

```js
{ path: '/admin', meta: { requiresAuth: true, role: 'admin' } }
```

---

### 5.3 Router Patterns

#### Dynamic Routing

Add or remove routes at runtime:

```js
router.addRoute({ path: '/plugin-page', component: PluginPage });
```

#### Lazy-Loaded Routes

```js
{
  path: '/dashboard',
  component: () => import('./views/Dashboard.vue') // code split
}
```

#### 404 Handling

```js
{ path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound }
```

#### Scroll Behavior

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) return savedPosition; // browser back/forward
    if (to.hash) return { el: to.hash };     // anchor link
    return { top: 0 };                       // scroll to top
  }
});
```

#### Route Meta Fields

Attach arbitrary data to routes for use in guards or components:

```js
{ path: '/settings', meta: { requiresAuth: true, title: 'Settings' } }
```

---

## 6. State Management

---

### 6.1 State Management Concepts

#### Local State vs Global State

- **Local state** lives in a single component (`ref`, `reactive`). Only that component cares about it.
- **Global state** is shared across multiple, unrelated components (user auth, theme, cart).

#### Prop Drilling

Passing state through many layers of intermediate components that don't need the data themselves, just to get it to a deeply nested child. This is painful to maintain.

#### Shared State Problems

Without a store, shared state tends to get duplicated, goes out of sync, and becomes hard to debug. Side effects become unpredictable.

#### When Global Store is Needed

- User authentication and profile.
- Shopping cart or multi-step form state.
- Theme/settings that affect the whole app.
- Data shared between sibling components at different levels of the tree.

---

### 6.2 Pinia

#### Why Pinia Replaced Vuex

- No mutations — simpler API (just state + getters + actions).
- Full TypeScript support out of the box.
- Devtools support.
- Modular by design — no nested modules.
- Works with Composition API naturally.

#### Store Definition (Setup Store style — recommended)

```js
// stores/counter.js
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0);
  const double = computed(() => count.value * 2);
  function increment() { count.value++; }
  return { count, double, increment };
});
```

#### State, Getters, Actions

```js
// Options store style
export const useUserStore = defineStore('user', {
  state: () => ({ name: '', isLoggedIn: false }),
  getters: {
    displayName: (state) => state.name || 'Guest',
  },
  actions: {
    async login(credentials) {
      const user = await api.login(credentials);
      this.name = user.name;
      this.isLoggedIn = true;
    }
  }
});
```

#### Using a Store in a Component

```js
import { useCounterStore } from '@/stores/counter';
const store = useCounterStore();
// Access: store.count, store.increment()
// Destructure with storeToRefs for reactivity:
const { count, double } = storeToRefs(store);
```

#### Async Actions

Actions can be async. Handle loading/error state inside the action or in the component.

#### Store Persistence Basics

Use `pinia-plugin-persistedstate` to automatically sync store state to `localStorage` or `sessionStorage`.

#### Testing Stores

Use `setActivePinia(createPinia())` in test setup. Test state, getters, and actions independently.

---

### 6.3 Vuex — Legacy Awareness

#### Core Concepts

- **State:** Single source of truth.
- **Mutations:** The ONLY way to change state synchronously. Must be synchronous.
- **Actions:** Commit mutations. Can be async. Used for API calls.
- **Getters:** Computed state derived from the store.
- **Modules:** Split store into namespaced modules for large apps.

```js
// Vuex pattern
store.commit('INCREMENT');          // mutation
store.dispatch('fetchUser', id);    // action
store.getters.fullName;            // getter
```

#### Difference from Pinia


|            | Vuex            | Pinia                        |
| ---------- | --------------- | ---------------------------- |
| Mutations  | Required        | Gone — directly mutate state |
| TypeScript | Painful         | First-class                  |
| Modules    | Nested, verbose | Flat stores, compose them    |
| Devtools   | Supported       | Supported + better           |
| Vue 3      | Works           | Designed for Vue 3           |


#### Migration Talking Points

"Vuex required a mutation to change any state, which added boilerplate. Pinia removes mutations entirely — actions can directly mutate state, which makes the code far simpler. Migration is straightforward because each Vuex module maps cleanly to a Pinia store."

---

## 7. Forms in Vue

---

### 7.1 Form Handling

#### `v-model` on Native Inputs

```html
<input v-model="name" type="text" />
<textarea v-model="bio" />
<input v-model="agreed" type="checkbox" />
<input v-model="gender" type="radio" value="male" />
<select v-model="country">
  <option value="pk">Pakistan</option>
</select>
```

#### `v-model` Modifiers

```html
<input v-model.trim="name" />     <!-- trims whitespace -->
<input v-model.number="age" />    <!-- converts to number -->
<input v-model.lazy="email" />    <!-- syncs on change, not input -->
```

#### Checkboxes

Single checkbox binds to a boolean. Multiple checkboxes with the same `v-model` bind to an array:

```html
<input type="checkbox" v-model="selectedFruits" value="apple" />
<input type="checkbox" v-model="selectedFruits" value="mango" />
```

#### Custom Component `v-model`

```vue
<!-- MyInput.vue -->
<script setup>
defineProps(['modelValue']);
const emit = defineEmits(['update:modelValue']);
</script>
<template>
  <input :value="modelValue" @input="emit('update:modelValue', $event.target.value)" />
</template>

<!-- Parent -->
<MyInput v-model="name" />
```

Multiple v-model bindings (Vue 3):

```html
<UserForm v-model:name="user.name" v-model:email="user.email" />
```

---

### 7.2 Validation

#### Manual Validation

```js
const errors = reactive({});
function validate() {
  errors.name = name.value ? '' : 'Name is required';
  errors.email = /\S+@\S+\.\S+/.test(email.value) ? '' : 'Invalid email';
  return !Object.values(errors).some(Boolean);
}
```

#### Schema Validation Concepts

Schema-based validation separates validation rules from component logic. A schema describes the shape and constraints of data. Libraries validate data against the schema and return errors.

#### Libraries — VeeValidate, Yup, Zod

- **VeeValidate:** Vue-specific. Has composables (`useField`, `useForm`) and component wrappers. Integrates with Yup/Zod for schema validation.
- **Yup:** JavaScript schema builder. Chainable, promise-based validation.
- **Zod:** TypeScript-first schema validation. Provides type inference.

```js
// With VeeValidate + Zod
import { useForm } from 'vee-validate';
import { toTypedSchema } from '@vee-validate/zod';
import { z } from 'zod';

const { handleSubmit, errors } = useForm({
  validationSchema: toTypedSchema(
    z.object({
      email: z.string().email(),
      age: z.number().min(18),
    })
  )
});
```

#### Error Display UX

- Show errors only after a field has been touched/blurred (not before the user interacts).
- Inline errors next to each field are better than a summary at the top.
- Use ARIA attributes (`aria-describedby`, `role="alert"`) for accessibility.
- Clear errors as the user corrects them.

#### Async Validation

For validations requiring a server round-trip (checking if a username is taken):

```js
const usernameAvailable = ref(true);
watch(username, async (val) => {
  const result = await api.checkUsername(val);
  usernameAvailable.value = result.available;
});
```

Debounce the watcher to avoid excessive API calls:

```js
import { useDebounceFn } from '@vueuse/core';
const checkUsername = useDebounceFn(async (val) => {
  usernameAvailable.value = await api.checkUsername(val);
}, 400);
watch(username, checkUsername);
```

---

## Quick Interview Cheat Sheet


| Topic                | Key Phrase to Remember                                                      |
| -------------------- | --------------------------------------------------------------------------- |
| Progressive          | "Incrementally adoptable — from script tag to full SPA"                     |
| Reactivity           | "Proxy-based — tracks reads, triggers on writes"                            |
| ref vs reactive      | "ref for primitives, reactive for objects, but ref works for both"          |
| computed             | "Cached derived state — only re-runs when deps change"                      |
| watch                | "Side effects on reactive changes — API calls, logging"                     |
| Composables          | "Reusable stateful logic via Composition API functions"                     |
| Props                | "Down only — child never mutates props"                                     |
| emit                 | "Up only — child notifies parent via events"                                |
| Pinia                | "No mutations, just state + getters + actions"                              |
| Route guards         | "beforeEach for auth checks globally"                                       |
| v-model              | "Shorthand for :value + @input (or :modelValue + @update)"                  |
| key                  | "Stable unique identity for list items — never use index for dynamic lists" |
| KeepAlive            | "Caches component instance — preserves state across tab switches"           |
| Teleport             | "Renders content elsewhere in DOM — for modals/tooltips"                    |
| defineAsyncComponent | "Lazy load — reduces initial bundle, loads on demand"                       |


