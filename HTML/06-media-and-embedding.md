# 06. Media and Embedding

## Images
Basic image example:
```html
<img src="photo.jpg" alt="A lake near mountains" />
```

## Responsive images
Sometimes different screen sizes need different image sizes.

### `srcset`
```html
<img
  src="small.jpg"
  srcset="small.jpg 480w, medium.jpg 800w, large.jpg 1200w"
  sizes="(max-width: 600px) 480px, 800px"
  alt="Product image"
/>
```

## `picture`
Used when you want different image sources for different conditions.
```html
<picture>
  <source media="(max-width: 600px)" srcset="mobile.jpg" />
  <source media="(max-width: 1024px)" srcset="tablet.jpg" />
  <img src="desktop.jpg" alt="Banner image" />
</picture>
```

## Lazy loading images
```html
<img src="image.jpg" alt="Example image" loading="lazy" />
```
This can improve performance by delaying off-screen images.

## Audio
```html
<audio controls>
  <source src="audio.mp3" type="audio/mpeg" />
  Your browser does not support audio.
</audio>
```

## Video
```html
<video controls width="400">
  <source src="movie.mp4" type="video/mp4" />
  Your browser does not support video.
</video>
```

## `iframe`
Used to embed another page or external content.
```html
<iframe src="https://example.com" title="Example site"></iframe>
```

### Important point
Always give `iframe` a useful `title` for accessibility.

## `figure` and `figcaption`
Used for media with a caption.
```html
<figure>
  <img src="chart.png" alt="Sales chart for 2025" />
  <figcaption>Sales chart for the year 2025</figcaption>
</figure>
```

## Interview questions
### What is `srcset` used for?
It helps serve different image files for different screen sizes or resolutions.

### Why use `loading="lazy"`?
To improve performance by delaying loading of off-screen images.

### Why should an iframe have a title?
It improves accessibility for screen reader users.

## Quick revision
- Learn `img`, `picture`, and `srcset`
- Learn audio and video basics
- Know what iframe does
- Remember `figure` and `figcaption`
