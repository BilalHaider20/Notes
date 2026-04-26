# 09. Advanced HTML Concepts

## `data-*` attributes
Custom data attributes store extra information on elements.
```html
<button data-user-id="42">View Profile</button>
```

## `details` and `summary`
Useful for collapsible content.
```html
<details>
  <summary>Read more</summary>
  <p>This content can be expanded.</p>
</details>
```

## `dialog`
Used for modal or popup content.
```html
<dialog open>
  <p>This is a dialog box.</p>
</dialog>
```

## `template`
Holds HTML that is not rendered immediately.
Useful with JavaScript.
```html
<template id="card-template">
  <div class="card">Card content</div>
</template>
```

## `noscript`
Shown when JavaScript is disabled.
```html
<noscript>Please enable JavaScript to use this app.</noscript>
```

## `contenteditable`
Makes content editable by the user.
```html
<div contenteditable="true">Edit this text</div>
```

## Progressive enhancement
Start with working HTML first.
Then improve experience with CSS and JavaScript.
This is a strong frontend mindset.

## Interview questions
### What are `data-*` attributes used for?
They store custom data on HTML elements.

### What is the purpose of `template`?
It stores HTML that is not displayed immediately and can be used later with JavaScript.

### What is progressive enhancement?
It means building a functional base experience first, then adding better styling and behavior.

## Quick revision
- Learn a few advanced tags
- Understand progressive enhancement
- Know `data-*`, `dialog`, `template`, and `noscript`
