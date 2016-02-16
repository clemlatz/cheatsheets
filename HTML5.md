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


### All together now

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

## HTML5 new inputs types

HTML5 introduces new form element. If a browser doesn't support an input type,
it defaults to text.

### Search

`<input type="search">` represents a one-line plain-text edit control for
entering one or more search terms. It features round corner and a cancel button.
On mobile, the keyboard default action button changes to "Search".

### Email

`<input type="email">` represents a one-line text for entering an e-mail adress.
A virtual keyboard will feature easy access to "@" and "." keys.

### URL

`<input type="url">` represents a one-line text for entering an http URL.
Anvirtual keyboard will feature easy access to "/", ".", or tld keys.

### Date

`<input type="date">` represents a one-line text for entering a string that
represents a date. It will often feature a date picker so we can easily enter a
date.

Also:
* `<input type="month">`
* `<input type="week">`
* `<input type="time">`
* `<input type="datetime-local">`

### Tel

`<input type="tel">` represents a one-line text for entering a string that
represents a phone number. Virtual keyboard will feature a phone keyboard.

### Number

`<input type="number">` represents a one-line precise control for entering a
string that represents a number. Virtual keyboard will feature a number only
keyboard.

### Range

`<input type="number">` represents a one-line imprecise control for entering a
string that represents a number. It will be rendered as a draggable slider.

### Color

`<input type="number">` lets us set a string that represents a color and will
be rendered as a color pickup.


## HTML5 new form elements

### Datalist

```html
<input type="text" list="browsers" />
<datalist id="browsers">
  <option value="Chrome">
  <option value="Edge">
  <option value="Firefox">
  <option value="Safari">
</datalist>
```

## HTML5 form attribute

### Placeholder

`placeholder` allow to specify a message shown before any text is entered.

```html
<input type="email" placeholder="Enter your email..." />
```

### Autofocus

An input with the autofocus attribute will automatically get focus when the
page is rendered.

```html
<input type="email" autofocus />
```

### Required

An input with the required attribute will notify the user if he tries to submit
the form with the field left blank.

```html
<input type="email" required />
```

### Pattern

The pattern attribute accepts a Javascript regular expression that can be used
to validate a form field to match the pattern.

```html
<input type="text" pattern="[0-9]{10}" />
```

## CSS 3

### border-radius property

Applies rounded border to corners.

`border-radius: <top left> <top right> <bottom left> <bottom right>`

```css
.box {
  border-radius: 4px 15px 12px 10px;
  border-radius: 50%;
}
```

### box-shadow property

Specifies a shadow on a element. Multiple shadows can be specified with a comma.

`box-shadow: <inset> <offset-x> <offset-y> <blur-radius> <spread-radius> <color>`

* `inset`: if the keyword is present, the shadow will be inside the element. If
not (by default), the element will have a drop shadow.
* `offset-x`: moves the shadow along the horizontal axis.
* `offset-y`: moves the shadow along the vertical axis.
* `blur-radius`: alters the blur amount of the shadow, causing it to become
bigger and lighter with a bigger value.
* `spread-radius`: causes the shadow to expand or shrink.
* `color`: the color of the shadow.

```css
.box {
  box-shadow: -1px 3px 2px #000, inset 3px 3px 1px #666;
}
```

### text-shadow property

Very similar to `box-shadow`, but applies the shadow to a text.

`box-shadow: <offset-x> <offset-y> <blur-radius> <color>`

### box-sizing property

The `box-sizing` property is used to change the default CSS box model, which
is used to calculate widths and heights of given elements.
Each HTML element is a box, which consists of *margins*, *borders*,
*padding* and the *content* of the element. The *box model* refers to how those
properties are calculated in conjunction with one another in order to set the
element's dimensions.
* `content-box` (default): the width and height are measured by including only
the content, but not the border, margin or padding.
* `padding-box`: includes content and padding, but not border or margin.
* `border-box`: includes content, padding and border, but not margin.

### Multiple backgrounds

CSS3 allows to apply multiple background, stacked in the order which they are
specified (first on top).

```css
.element {
  background-image: url(bg1.png), url(bg2.png);
  background-position: top left, center right;
  background-repeat: no-repeat, no-repeat;
}
```

### Colors

* `rgba()` represents the three additive primary colors (red, green, blue), plus
a fourth value (alpha) for the opacity.
* `hsla()` represents hue, saturation and lightness, plus a fourth value for the
the opacity.

```css
.element {
  background-color: rgba(255, 0, 0, .65);
  color: hsla(240, 100%, 50%, .25);
}
```

### Opacity

Allows to specify the opacity of an element. It also affects the element nested
inside.

```css
.element {
  opacity: .45;
}
```

### Linear gradients

Allows to create smooth transitions between two or more colors. The are create
by specifying starting point, ending point and optional stop-color points.

`linear-gradient(<angle>, <color-stop>s)`

* `angle` is generally in degrees (eg. `45deg`) or a keyword (`to top`: 0deg,
`to bottom`: 180deg, `to right`: 270deg, `to left`: 90deg)
* `color-stop` consists of a color and optional stop position, which can be
either a percentage or a length

```css
.element {
  background: linear-gradient(to bottom, red, yellow)
}
```

### Radial gradient

A `radial-gradient`, unlike `linear-gradient`, creates a gradient thats extends
from an origin, the center of an element, extending outward in a circular or
ellpitic shape.

`radial-gradient(<shape> <size> at <position>, <color-stop>)`

* `shape` can be `circle` or `ellipsis` (default)
* `size` can be `closest-side`, `closest-corner`, `farthest-side`,
`farthest-corner` (default) or a length or percentage
* `position` default is `center` but all `background-position` values can be
used here
* `color-stop` consists of a color and optional stop position, which can be
either a percentage or a length

```css
.element {
  background: radial-gradient(aqua, blue);
}
```

## Fonts

### Font-face

`font-face` lets us use online fonts in a web page.

```css
@font-face {
  font-family: 'OpenSansRegular';
  src: url('OpenSansRegular-webfont.eot');
  font-style: normal;
  font-weight: normal;
}

@font-face {
  font-family: 'OpenSansRegular';
  src: url('OpenSansBold-webfont.eot');
  font-style: normal;
  font-weight: bold;
}

p {
  font-family: 'OpenSansRegular', Helvetica, sans-serif;
}

p.bold {
  font-weight: bold; /* Will use OpenSansBold version */
}
```


## Transform

Allows to translate, rotate, scale and skew elements with CSS.

```css
.element {
  transform: translate(20px, 30px),
    rotate(45deg),
    scale(1.2),
    skewX(-2.5deg);
}
```

### Translate

`translate(<tx>, <ty>)`
`translateX(<tx>)`
`translateY(<ty>)`

`<tx>` and `<ty>` can either be length or percentage

### Rotate

Rotates an element clockwise from its origin.

`rotate(<angle>)`

### Scale

Scales an element by specify a unitless number.

`scale(<sx>, <sy>)`
`scaleX(<sx>)`
`scaleT(<sy>)`

If `<sy>` is not specified, it will default to the value of `<sx>`

### Skew

Skews an element around the x or y axys by the specified angle.

`skewX(<ax>)`
`skewY(<ay>)`


## Transitions

Allows to transition between two states of an element.

`transition: <property> <duration> <timing-function> <delay>`

* `property`: the CSS property affected by the transition (or `all`)
* `duration`: the amount of time we want the transition to take place
* `timing-function`: can either be `ease`, `ease-in`, `ease-in-out`,
  `linear`, `cubic-bezier`, `step-start`, `step-end`, `steps()`
* `delay`: the amount of time until the transition starts

```css
.element {
  background-color: black;
  transition: background-color .2s ease-in-out;
  transition-property: background-color;
  transition-duration: .2s;
  transition-timing-function: ease;
  transition-delay: .1s;
}

.element:hover {
  background-color: blue;
}
```

## Progressive enhancement

The term refers to the use of newer features that add to the experience in
modern browsers without detracting from the experience in older browsers.