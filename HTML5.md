# HTML5


## Updated elements

### Doctype

```html
<!DOCTYPE html>
```

### Meta declaration

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
```

becomes

```html
<meta charset="UTF-8">
```

### Script tag

The `type` attribute is no longer mandatory.

```html
<script src="./script.js"></script>
```

### Link tag

The `type` attribute is no longer mandatory.

```html
<link rel="styleshee" href="./styles.css">
```

### Emphasis and importance

* `<i>` indicates an alternate voice or mood
* `<b>` indicates a stylistically offset
* `<em>` indicates a stress emphasis
* `<strong>` indicates a strong importance


## HTML5 new elements

### Document outline

In HTML5, some elements (`<article>`, `<aside>`, `<nav>` and `<section>`) have their own self-contained outline.

```html
<h1>Title of the page
<section>
  <h2>Title of the section</h2>
</section>
```

has the same outline than

```html
<h1>Title of the page
<section>
  <h1>Title of the section</h1>
</section>
```

which is :
1. Title of the page  
1.1 Title of the section

### Section element

`<section>` represents a generic document or application section and, unlike
`<div>`, has a semantic meaning. It's used for grouping together thematically
related content.

### Header element

`<header>` represents a group of introductory or navigational aids.

### Footer element

`<footer>` represents a footer for its nearest ancestor sectioning content.

Headers and footers are not position-dependant, they should describe the content
they are contained within.

### Aside element

`<aside>` orginally represents a content that is tangentially (not directly)
related to the content surrounding it. Now :
* Inside an `<article>` element, it relates to that article
* Outside of an `<article>`, it relates to the site as a whole (e.g. a sidebar)
It's appropriate when its content is not the primary focus of an article or page
but still related to its container.

### Nav element

`<nav>` represents a section with navigation links to other pages or to parts
within the same page.

### Article element

`<article>` represents a complete or self-contained composition in a document,
page, application or site that is, in principle, independently distributable
or reusable. Examples: a blog post, a news story, a comment, a product review.

### Main element

`<main>` represents the main content of the body of a document or application.
There should not be more than one `<main>` and it should not be included inside
of an `<article>`, `<aside>`, `<footer>`, `<header>` or `<nav>` element.

### Figure and figcaption elements

`<figure>` represents a unit a content that is self-contained and would not
affect the meaning of the document if removed. A common use case is for an image
within an article.
`<figcaption>` represents a caption or a legend for its parent figure.

### Time element

`<time>` represents either a time on a 24 hour clock, or a precise date, optionally
with a time and a time-zone offset. If not in those format, it should use the
`datetime` attribute.

```html
<nav>
  <ul>
    <!-- site navigation -->
  </ul>
</nav>
<main>
  <section>
    <header>
      <h1>The heading of the section</h1>
      <p>This is content in the header.</p>
    </header>
    <p>This is content within the section.</p>
    <article>
      <figure>
        <img src="image.jpg" alt="An image">
        <figcaption>The caption of the parent figure.</figcaption>
      </figure>
      <p>This is the content of the article.</p>
      <time datetime="2013-05-22">22/05/2013</time>
    </article>
    <aside>
      Some secondary information.
    </aside>
    <footer>
      <p>This is the footer's content.</p>
    </footer>
  </section>
</main>
```