# 05. Accessibility Basics

## What is accessibility?
Accessibility means building websites that more people can use, including users with disabilities.

This is a very important interview topic.

## Why accessibility matters
- helps screen reader users
- helps keyboard-only users
- improves usability for everyone
- often legally and professionally important

## Basic accessibility rules
- use semantic HTML
- use proper heading order
- add `alt` text to images
- connect `label` with form inputs
- make sure buttons are real buttons
- ensure keyboard access

## `alt` text
```html
<img src="team.jpg" alt="Development team sitting in a meeting room" />
```

### Good `alt` text
Describe meaningful content.

### Empty alt
```html
<img src="decoration.png" alt="" />
```
Use empty alt for decorative images.

## Headings and structure
Use headings in order.
Do not skip levels without reason.
This helps screen reader navigation.

## `button` vs `a`
### Use `button` when:
- performing an action
- opening a modal
- submitting a form

### Use `a` when:
- navigating to another page or location

## Form accessibility
Always connect label and input.
```html
<label for="username">Username</label>
<input id="username" name="username" type="text" />
```

## Keyboard accessibility
Users should be able to use the site without a mouse.
Make sure interactive elements are reachable using Tab.

## `tabindex`
Used to control keyboard focus.
Do not overuse it.
In most cases, native HTML elements already handle focus correctly.

## ARIA basics
ARIA stands for **Accessible Rich Internet Applications**.
It helps provide extra accessibility information.

Examples:
- `aria-label`
- `aria-hidden`
- `role`

### Important rule
Use semantic HTML first.
Use ARIA only when needed.

## Interview questions
### Why is semantic HTML good for accessibility?
Because it helps assistive technologies understand the structure and meaning of content.

### When should you use a button instead of a link?
Use a button for actions and a link for navigation.

### What is `alt` text?
Alternative text that describes an image for accessibility and fallback.

### What is ARIA?
A set of attributes that improve accessibility when native HTML is not enough.

## Quick revision
- Accessibility is not optional in modern frontend
- Use semantic HTML first
- Use proper labels, headings, and alt text
- Know button vs link clearly
