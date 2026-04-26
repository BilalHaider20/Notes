# 02. Vue Fundamentals

## 2.1 What Vue Is
Vue is a progressive JavaScript framework for building user interfaces.

### Key ideas
- Declarative rendering
- Reactive state management
- Component-based architecture
- Easy gradual adoption

### Why companies use Vue
- Clean learning curve
- Good developer experience
- Strong ecosystem
- Suitable for both small and large applications

## 2.2 Single File Components
Vue commonly uses `.vue` files.

```vue
<template>
  <h1>{{ title }}</h1>
</template>

<script setup>
const title = 'Hello Vue'
</script>

<style scoped>
h1 {
  color: blue;
}
</style>
```

### Main sections
- `<template>`: UI markup
- `<script>` or `<script setup>`: logic
- `<style>`: component styles

### Benefits
- Keeps related code together
- Easier component maintenance
- Better readability

## 2.3 Template Syntax

### Interpolation
```vue
<p>{{ message }}</p>
```
Used to display reactive data.

### Attribute binding with `v-bind`
```vue
<img :src="imageUrl" :alt="title" />
```
Shorthand for `v-bind:` is `:`.

### Event binding with `v-on`
```vue
<button @click="increment">Add</button>
```
Shorthand for `v-on:` is `@`.

### Two-way binding with `v-model`
```vue
<input v-model="name" />
```
This keeps input value and state in sync.

### Conditional rendering
- `v-if`
- `v-else-if`
- `v-else`
- `v-show`

**Difference:**
- `v-if` adds/removes from DOM
- `v-show` toggles visibility with CSS

### List rendering with `v-for`
```vue
<li v-for="user in users" :key="user.id">{{ user.name }}</li>
```

### Why `key` matters
Vue uses `key` to track element identity when rendering lists.
Bad keys can cause:
- incorrect DOM reuse
- state bugs in forms
- rendering inconsistencies

Use stable unique keys, usually database ids.

## 2.4 Reactivity in Vue

### What reactivity means
When data changes, Vue updates the UI automatically.

### Why it matters
Developers describe the desired state, and Vue keeps the DOM in sync.

### High-level mechanism
Vue tracks dependencies used during rendering or computed evaluation. When reactive data changes, Vue updates only what depends on it.

## 2.5 Components

### Why components exist
Components make UI reusable, isolated, and easier to test.

### Good component design
- small and focused
- reusable where appropriate
- clear inputs and outputs
- minimal side effects

## 2.6 Props
Props are how parent components pass data to children.

```vue
<script setup>
defineProps({
  title: String,
  count: Number
})
</script>
```

### Rules
- Props are read-only in child components
- Data flows downward from parent to child
- Child should not mutate props directly

### Common interview point
If child needs editable local state based on a prop, create local state initialized from the prop instead of mutating the prop.

## 2.7 Events and Emits
Children communicate upward by emitting events.

```vue
<script setup>
const emit = defineEmits(['save'])

function handleClick() {
  emit('save')
}
</script>
```

Parent usage:
```vue
<ChildComponent @save="saveData" />
```

### Props vs emits
- Props: parent to child
- Emits: child to parent

This is core to Vue’s one-way data flow.

## 2.8 Slots
Slots allow a parent to pass markup into a child component.

### Default slot
```vue
<Card>
  <p>Content goes here</p>
</Card>
```

### Named slots
```vue
<Card>
  <template #header>
    <h2>Title</h2>
  </template>
</Card>
```

### Scoped slots
Used when child provides data back to slot content.

**Interview angle:**
Slots are useful for flexible reusable components such as modals, tables, cards, and layouts.

## 2.9 Event Modifiers and Input Modifiers
Examples:
- `@click.prevent`
- `@submit.prevent`
- `@click.stop`
- `v-model.trim`
- `v-model.number`
- `v-model.lazy`

These reduce manual boilerplate.

## 2.10 Provide and Inject
A way to share data/functions across deeply nested components without prop drilling.

### Use cases
- theme data
- form groups
- plugin-like shared dependencies

### Caution
Do not use it as a full replacement for state management.

## Common mistakes
- Mutating props directly
- Using array index as `key` in dynamic lists
- Overusing global state for local problems
- Putting too much logic directly in templates
- Using `v-if` and `v-for` carelessly on the same element

## Interview quick answers

### What is Vue?
A progressive framework for building reactive, component-based user interfaces.

### What are props?
Inputs passed from parent to child components.

### What are emits?
Custom events emitted by child components to notify parents.

### Difference between `v-if` and `v-show`?
`v-if` conditionally creates/removes DOM. `v-show` always renders and toggles visibility with CSS.

### Why is `key` important in `v-for`?
It helps Vue correctly track and update list items, preventing rendering and state bugs.
