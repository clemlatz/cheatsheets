# React

React is a JavaScript library for building user interfaces, created by Facebook.
Some people use it as the V in MVC. It was built to solve one problem: building
large applications with data that changes over time.

## First component

React uses a component-based architecture. A component generates HTML using
the `render()` method.

The virtual DOM is an in-memory representation of real DOM elements generated
by React components before any changes are made to the page. A component
generated in-memory virtual DOM elements that will later become actual HTML
elements displayed by the browser.

React is fast because it can *diff* the virtual DOM and modify only the real
DOM elements that have changed, while other elements remained untouched.

Components in React are JavaScript classes that inherits from the
`React.Component` base class. Every components needs at least a `render()`
method.

The `ReactDOM.render` method render our component on the page.

```javascript
class BookshelfBox extends React.Component {
  render() {
    return( <div>Bookshelf Box</div> );
  }
}

ReactDOM.render(
  <BookshelfBox />, document.querySelector('#bookself-app');
)
```

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="bookself-app"></div>
  </body>
  <script src="vendors/react.js"></script>
  <script src="vendors/react-dom.js"></script>
  <script src="vendors/babel.js"></script>
  <script type="text/babel" src="components.js"></script>
</html>
```


## Understanding JSX

The markup we use when writing React apps is not a string. JSX stands for
JavaScript XML. JSX is just another way of writing JavaScript with a transpile
step.

### Elements written in lowercase are rendered as normal HTML elements.

```javascript
class BookshelfBox extends React.Component {
  render() {
    return( <div>
      <h3>Bookshelf Box</h3>
      <p className="lead">Sample paragraph</p>
    </div> );
  }
}
```

`className` is used for HTML attribute `class` because `class` is a JavaScript
reserved keyword.

Once transpiled, the HTML above becomes:

```javascript
React.createElement('div', null,
  React.createElement('h3', null, "Bookshelf App"),
  React.createElement('p', { className: "lead" }, "Sample paragraph")
);
```

### Elements in upper camel case are rendered as React components.

```javascript
ReactDOM.render(
  <BookshelfBox />, document.querySelector('#bookself-app');
)
```

`<BookshelfBox />`, once transpiled, becomes:
`React.createElement(BookshelfBox, null)`


### Using JavaScript code in JSX templates

In JSX, code written within curly braces gets interpreted as literal JavaScript.

```javascript
class BookshelfBox extends React.Component {
  render() {
    const topicsList = ['HTML', 'JavaScript', 'React'];
    return( <div>
      <h3>Bookshelf Box</h3>
      Topics:
      <ul>
        {topicsList.map(topic => <li>{topic}</li>)}
      </u>
    </div> );
  }
}
```
