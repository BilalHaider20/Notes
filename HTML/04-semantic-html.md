# 04. Semantic HTML

## What is semantic HTML?
Semantic HTML means using tags that clearly describe their purpose.

Instead of using only many `div` tags, we use meaningful tags like:
- `header`
- `nav`
- `main`
- `section`
- `article`
- `aside`
- `footer`

## Why semantic HTML matters
- improves readability of code
- helps accessibility
- helps SEO
- makes maintenance easier

## Common semantic elements
### `header`
Top area of a page or section.

### `nav`
Contains navigation links.

### `main`
Contains the main content of the page.
There should usually be only one `main` per page.

### `section`
Groups related content.

### `article`
Represents independent content, like a blog post or news card.

### `aside`
Contains side content, related info, or sidebar content.

### `footer`
Bottom area of a page or section.

## Example layout
```html
<body>
  <header>
    <h1>My Website</h1>
    <nav>
      <a href="#">Home</a>
      <a href="#">About</a>
    </nav>
  </header>

  <main>
    <section>
      <h2>Featured Articles</h2>
      <article>
        <h3>Post Title</h3>
        <p>Post content...</p>
      </article>
    </section>

    <aside>
      <p>Related links</p>
    </aside>
  </main>

  <footer>
    <p>Copyright 2026</p>
  </footer>
</body>
```

## `div` vs semantic tags
### `div`
A generic container with no meaning.
Use it when no semantic tag fits.

### Semantic tags
They tell the browser and developers what the content means.

## `section` vs `article`
### `section`
Groups related content into a theme.

### `article`
A self-contained piece of content that can stand alone.

## Common mistakes
- using too many `div`s for everything
- using semantic tags without logical structure
- using headings badly inside sections

## Interview questions
### Why is semantic HTML important?
It improves accessibility, SEO, readability, and maintainability.

### What is the difference between `section` and `article`?
A `section` groups related content, while an `article` is self-contained content.

### When should you use `div`?
Use `div` when no semantic element matches the purpose.

## Quick revision
- Prefer meaningful tags over generic containers
- Use `main` for main content
- Use `nav` for navigation
- Know `section` vs `article`
