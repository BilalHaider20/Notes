# 02. Text, Links, Images, and Lists

## Headings
HTML provides 6 heading levels.
```html
<h1>Main Title</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>
```

### Important rule
Use only one main `h1` in most pages.
Keep heading order logical.
Do not jump from `h1` to `h4` without reason.

## Paragraphs
```html
<p>This is a paragraph.</p>
```

## Text formatting tags
- `strong` for important text
- `em` for emphasized text
- `mark` for highlighted text
- `small` for smaller text
- `del` for deleted text
- `ins` for inserted text

Example:
```html
<p><strong>Important:</strong> Read this carefully.</p>
```

## Links
```html
<a href="https://example.com">Open Example</a>
```

### Common link attributes
- `href` = destination
- `target="_blank"` = open in new tab
- `rel="noopener noreferrer"` = safer when opening new tab

Example:
```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">Visit Site</a>
```

## Absolute vs relative paths
### Absolute path
Full URL.
```html
<a href="https://example.com/about">About</a>
```

### Relative path
Path inside your project.
```html
<a href="about.html">About</a>
```

## Images
```html
<img src="photo.jpg" alt="A mountain view" />
```

### Important image attributes
- `src` = image location
- `alt` = alternative text
- `width` and `height` can also be used

### Why `alt` matters
- helps screen reader users
- useful if image fails to load
- good for accessibility and SEO

## Lists
### Unordered list
```html
<ul>
  <li>HTML</li>
  <li>CSS</li>
  <li>JavaScript</li>
</ul>
```

### Ordered list
```html
<ol>
  <li>Learn basics</li>
  <li>Practice code</li>
  <li>Revise interview questions</li>
</ol>
```

### Description list
```html
<dl>
  <dt>HTML</dt>
  <dd>Used for page structure</dd>
</dl>
```

## `div` vs `span`
### `div`
A block-level container.
Used to group larger sections.

### `span`
An inline container.
Used for small pieces of text.

## Easy interview questions
### What is the difference between `strong` and `b`?
`strong` gives semantic importance, while `b` is mostly visual bold text.

### What is the difference between `em` and `i`?
`em` adds semantic emphasis, while `i` is mostly visual italic text.

### Why is `alt` important in images?
It improves accessibility and helps when the image cannot load.

### What is the difference between absolute and relative path?
- Absolute path is a full URL
- Relative path points to files inside the project

## Quick revision
- Use proper heading order
- Always add `alt` to images
- Use semantic text tags when possible
- Know how links and paths work
