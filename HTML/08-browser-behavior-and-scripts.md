# 08. Browser Behavior and Scripts

## How the browser reads HTML
The browser parses HTML from top to bottom and builds the DOM.
DOM stands for **Document Object Model**.
It is a tree-like structure of the page.

## What is the DOM?
The DOM is the browser's object representation of the HTML document.
JavaScript can read and change the DOM.

## Attributes vs properties
### Attributes
Written in HTML.
Example:
```html
<input type="text" value="Bilal" />
```

### Properties
Represent current values in the DOM object.
They can change with JavaScript or user interaction.

## Script loading
### Normal script
```html
<script src="app.js"></script>
```
This can block HTML parsing while loading and executing.

### `defer`
```html
<script src="app.js" defer></script>
```
- downloads in parallel
- executes after HTML parsing
- keeps order for deferred scripts

### `async`
```html
<script src="app.js" async></script>
```
- downloads in parallel
- executes as soon as ready
- may not keep order

## `defer` vs `async`
### Use `defer` when:
- script depends on DOM
- script order matters

### Use `async` when:
- script is independent
- order does not matter, like analytics

## Why script location matters
Older practice was putting scripts before closing `body`.
Modern practice often uses `defer` in `head`.

## Interview questions
### What is DOM?
DOM is the browser's tree representation of the HTML document.

### What is the difference between `async` and `defer`?
`defer` waits until HTML parsing is complete, while `async` runs as soon as the file is ready.

### Why can a normal script block rendering?
Because the browser may stop HTML parsing to load and run the script.

## Quick revision
- Learn what DOM is
- Learn browser parsing at a basic level
- Understand `defer` and `async` clearly
