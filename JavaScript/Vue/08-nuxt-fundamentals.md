# 08. Nuxt Fundamentals

## 8.1 What Nuxt Is
Nuxt is a framework built on top of Vue that adds application structure, routing, server rendering, and full-stack capabilities.

### Why Nuxt matters in interviews
Nuxt is often chosen when teams need:
- SSR for SEO and performance
- file-based routing
- simplified data fetching
- backend endpoints in the same project
- better conventions for scaling apps

## 8.2 Rendering Modes

### CSR: Client-Side Rendering
The browser loads JavaScript and renders the app mostly on the client.

**Pros:**
- simpler hosting
- rich SPA behavior

**Cons:**
- weaker initial SEO unless handled carefully
- slower first meaningful content on some devices

### SSR: Server-Side Rendering
HTML is generated on the server for the initial request.

**Pros:**
- better SEO
- faster first content for users
- better social sharing previews

**Cons:**
- more server complexity
- hydration issues can happen

### SSG: Static Site Generation
Pages are pre-generated at build time.

**Pros:**
- fast delivery
- CDN-friendly
- great for content-heavy sites

**Cons:**
- less flexible for highly dynamic data

### Hybrid rendering
Different routes can use different strategies depending on the app’s needs.

## 8.3 Nuxt Project Structure

### Important directories
- `pages/`: defines routes automatically
- `components/`: reusable UI pieces
- `layouts/`: shared page shells
- `composables/`: reusable Composition API logic
- `plugins/`: app-wide injections and setup
- `middleware/`: route guards and navigation logic
- `server/`: API routes and server logic
- `public/`: static public assets

### Important files
- `app.vue`: root app component
- `nuxt.config.ts`: app configuration

## 8.4 File-based Routing
Nuxt creates routes based on files inside `pages/`.

Examples:
- `pages/index.vue` -> `/`
- `pages/about.vue` -> `/about`
- `pages/users/[id].vue` -> `/users/:id`
- `pages/blog/[...slug].vue` -> catch-all route

### Why interviewers care
It shows you understand convention-over-configuration and can build routes quickly without manual router setup.

## 8.5 Layouts
Layouts let multiple pages share a common wrapper.

Examples:
- default layout
- auth layout
- dashboard layout

### Use cases
- shared headers and sidebars
- consistent page shell across sections
- different structure for public vs admin pages

## 8.6 Components in Nuxt
Nuxt supports component auto-importing.

### Benefits
- less manual import boilerplate
- cleaner code

### Caveat
You still need consistent naming and structure so the project stays understandable.

## 8.7 Composables in Nuxt
Composables in `composables/` are auto-imported in many setups.

### Common examples
- `useAuth`
- `useApi`
- `useUser`
- `useTheme`

These help centralize shared logic.

## 8.8 Why Nuxt is different from plain Vue
Plain Vue gives you the UI framework.
Nuxt gives you:
- app conventions
- SSR and SSG options
- built-in routing
- server endpoints
- runtime config
- middleware support

## Common mistakes
- Treating Nuxt exactly like a plain SPA
- Forgetting code may run on both server and client
- Using browser APIs during server rendering
- Poor file structure in large apps

## Interview quick answers

### What is Nuxt?
A Vue framework that provides file-based routing, SSR/SSG support, server endpoints, and production-ready app structure.

### Difference between Vue and Nuxt?
Vue is the UI framework. Nuxt is the higher-level framework built on top of Vue with routing, rendering modes, server utilities, and conventions.

### What is file-based routing?
Routes are created automatically from files inside the `pages` directory.

### Why use SSR in Nuxt?
For better SEO, faster initial content rendering, and improved link preview metadata.
