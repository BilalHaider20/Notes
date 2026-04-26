# 01. HTML Basics

## What is HTML?
HTML stands for **HyperText Markup Language**.
It is used to create the **structure of web pages**.

Think of HTML as the **skeleton** of a website.
- HTML gives structure
- CSS gives style
- JavaScript gives behavior

## Why HTML matters in interviews
Interviewers often check whether you:
- understand page structure
- know common tags
- use semantic elements correctly
- can build accessible markup

## Basic HTML document structure
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Page</title>
  </head>
  <body>
    <h1>Hello World</h1>
    <p>This is a basic HTML page.</p>
  </body>
</html>
```

## Important parts
### `<!DOCTYPE html>`
Tells the browser that this is an HTML5 document.

### `<html>`
The root element of the page.

### `<head>`
Contains metadata, page title, links to CSS, SEO tags, etc.
It does **not** show main visible content.

### `<body>`
Contains all visible content shown on the page.

## Common beginner tags
- `h1` to `h6` for headings
- `p` for paragraph
- `a` for links
- `img` for images
- `ul`, `ol`, `li` for lists
- `div` for grouping content
- `span` for small inline grouping
- `br` for line break
- `hr` for horizontal line

## Block vs inline elements
### Block elements
They take full width by default.
Examples:
- `div`
- `p`
- `h1`
- `section`

### Inline elements
They only take as much width as needed.
Examples:
- `span`
- `a`
- `strong`
- `em`

## Attributes
Attributes give extra information to tags.

Example:
```html
<a href="https://example.com" target="_blank">Visit</a>
```

Here:
- `href` tells where the link goes
- `target="_blank"` opens it in a new tab

## Comments in HTML
```html
<!-- This is a comment -->
```
Comments are ignored by the browser.

## Best practices
- Always use proper indentation
- Use lowercase tag names
- Add `lang` in the `html` tag
- Keep structure clean and readable
- Do not use HTML only for styling

## Easy interview questions
### What is HTML?
HTML is a markup language used to structure content on the web.

### What is the difference between HTML, CSS, and JavaScript?
- HTML = structure
- CSS = styling
- JavaScript = interactivity

### What is `<!DOCTYPE html>`?
It declares the document type and tells the browser to use HTML5.

### What is the difference between `head` and `body`?
- `head` contains metadata and setup information
- `body` contains visible page content

## Quick revision
- HTML builds structure
- Every page has `html`, `head`, and `body`
- Tags can have attributes
- Learn common tags first
