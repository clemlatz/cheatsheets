# React

React is a JavaScript library for building user interfaces, created by Facebook.
Some people use it as the V in MVC. It was built to solve one problem: building
large applications with data that changes over time.

## First component

React uses a component-based architecture. A **component** generates HTML using
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

A simple component can also be created from a simple function, also known as
functional components:

```javascript
const Book = (props) => {
  return <Book>{props.title}</Book>;
}
```

It's a best practice to use a functional component whenever our component
doesn't need to manage a state.

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
      </ul>
    </div> );
  }
}
```

### Using React Fragment to render multiple elements

In `render()` we should always return a single element (or an array of elements
with keys). In we want to return multiple elements, we can wrap them in another
element:

```jsx
render() {
  return (
    <div>
      <h1>Books</h1>
      <p>My bookshelf</p>
    </div>
  );
}
```

But if we doesn't want to pollute the DOM with unnecessary `<div>`, we can
return a `React.Fragment` (react 16.2+) component containing our elements.

```jsx
render() {
  return (
    <React.Fragment>
      <h1>Books</h1>
      <p>My bookshelf</p>
    </React.Fragment>
  );
}
```


## Props

React components can take arguments, called **props**, passed as HTML
attributes.

```javascript
class Bookshelf extends React.Component {

  _getBooks() { // _ helps distinguish custom methods from React methods
    const bookList = [ // usually obtained via xhr
      { id: 1, author: "Thomas Ligotti", title: "Chants du cauchemar et de la nuit" },
      { id: 2, author: "Lisa Tuttle", title: "Ainsi naissent les fantômes" }
    ]

    return bookList.map((book) => {
      return (
        <Book author={book.author} title={book.title} key={book.id} />
      );
    });
  }

  _getBookshelfTitle(booksCount) {
    if (booksCount === 0) {
      return 'Empty bookshelf';
    } else if (booksCount === 1) {
      return 'One book in bookshelf';
    } else {
      return `${booksCount} books in bookshelf`;
    }
  }

  render() {
    const books = this._getBooks();
    return(
    <div className="bookshelf">
      <h3>{this._getBookshelfTitle(books.length)}</h3>
      {books} // JSX knows how to render an array
    </div> );
  }
}
```

### Special `key` prop

The `key` prop is a special prop that helps React keep track of which element
is which in the loop. It can help improve performance.

From within the component, arguments can be accessed with the *this.props*
object.

```javascript
class Books extends React.Component {
  render() {
    return(
      <div className="book">
        <h3 className="book-title">{this.props.title}</h3>
        <p className="book-author">by {this.props.author}</p>
      </div>
    );
  }
}
```

### Special `children` prop

The `children` prop can be use to access the content of the component element
when it was called:

```javascript
function Book(props) {
  return <div className="book">{props.children}</div>;
}

ReactDOM.render(
  <Book>Catcher in the rye</Book>,
  document.querySelector('book')
);
```

## State

In React, we don't modify the DOM directly. Instead, we modify a component
state object in response to user events and let React handle updates to the
DOM.

The `state` object cannot be modified directly, instead we can use the
`setState` method. Calling it will only replace the properties passed as an
argument, not the entire state object.

```javascript
this.setState({ showBooks: true });
```

State properties must be initialized with default values in our class contructor.

```javascript
class Bookshelf extends React.Component {
  constructor() {
    super(); // must be called in our constructor

    this.state = {
      showBooks: false
    }
  }

  render() {
    let buttonText = 'Show books';

    if (this.state.showBooks) {
      buttonText = 'Hide books';
    }

    return (
      //...
      <button onClick={this._handleClick.bind(this)}>{buttonText}</button>
      //...
    );
    //...
  }

  _handleClick() {
    this.setState({
      showBooks: !this.state.showBooks
    });
  }
}
```

### prevState

Because state updates can happen asynchronously, when the state update is
based on the previous state (eg. incrementing a counter, negating a boolean),
it's better to use a callback with a `prevState` argument.

```javascript
this.setState(prevState => {
  return { someToggle: !prevState.someToggle }
});
```

### Updating arrays or objects in state

```javascript
const books = this.state.books;
books.splice(0, 1);
this.setState({ books });
```

This would work but is considered a bad practice because, as `books` is a
reference pointing to the same object than `this.state.books`, it would change
the `state` object directly and thus lead to unpredictable behavior.
A better way to do it is to clone the original object, ie. using the spread
operator, and pass the clone to `setState`.

```javascript
const books = [...this.state.books];
books.splice(0,1);
this.setState({ books });
```

## Refs

**Refs** allow to reference elements like form inputs in a Component from
anywhere within the class scope.

A `BookForm` component will handle adding new books, and will live inside the
main `Bookshelf` component.

```javascript
class BookForm extends React.Component {
  render() {
    return (
      <form onSubmit={this._handleSubmit.bind(this)}>
        <input placeholder="Title" ref={(input) => this._title = input}>
        <input placeholder="Author" ref={(input) => this._author = input}>
        <button>Validation></button>
      </form>
    )
  }
  _handleSubmit(event) {
    event.preventDefault();

    let title  = this._title; // refs to inputs from JSX
    let author = this._author;

    this.props.addBook(title.value, author.value);
  }
}
```

The `addBook` method called in the form component will be defined and passed
as a prop from within the `Bookshelf` component.

```javascript
class Bookshelf extends React.Component {

  constructor() {
    super();

    this.state = {
      showBooks: false,
      books: [
        { id: 1, author: "Thomas Ligotti", title: "Chants du cauchemar et de la nuit" },
        { id: 2, author: "Lisa Tuttle", title: "Ainsi naissent les fantômes" }
      ]
    }
  }

  render() {
    return (
      <div className="bookshelf">
        <BookForm addBook={this._addBook.bind(this)} />
      </div>
    )
  }

  _getBooks() {
    return this.state.books.map((book) => {
      return (
        <Book title={book.title} author={book.author} key={book.id} />
      );
    });
  }

  _addBook(title, author) {
    const book = {
      id: this.state.books.length + 1,
      title,
      author
    }
    this.setState({ books: this.state.books.concat([book]) });
  }
}
```

The use of concat (instead of push) helps React to stay fast, because it creates
and references a new array instead of updating the existing one.


## Lifecycle methods

Lifecycle methods are called when the Component is rendered for this first time
or about to be removed from the DOM.

1. `constructor()`
2. `componentWillMount()`: called before the component is rendered
3. `render()`
4. `componentDidMount()`: called after it is rendered
5. `componentWillUnmount()`: called before it is removed from the DOM

[Full method list](https://facebook.github.io/react/docs/component-specs.html#lifecycle-methods)

```javascript
class Bookshelf extends React.Component {

  // When component is rendered for the first time
  componentWillMount() {
    _fetchBooks();
  }

  // After component has been rendered
  componentDidMount() {
    this._timer = setInterval(() => this._fetchBooks(), 5000);
  }

  // When the component is about to be removed from the DOM
  componentWillUnmount() {
    clearInterval(this._timer);
  }

  _fetchBooks() {
    // Get books from server through XHR...
    fetch('/api/books')
      .then(function(books) {
        this.setState({ books });        
      });
  }
  //...
}
```

Since `_fetchBooks` calls the `setState` method that will trigger the
`render` method, calling it from within the `render` method will cause
an infinite loop. The `componentWillMount` method is more appropriate.

The `componentWillMount` can be used to clear memory, in order to avoid leaks.

### Creation lifecycle methods

When a component is created, the following lifecycle methods are called:

1. `constructor(props)` needs to call `super(props)`. It can set up state
but it shouldn't cause side-effects (eg. fetch data from the web).
2. `componentWillMount()` can update state and make last-minute
optimization, but shouldn't cause side-effects.
3. `render()` prepares and structures JSX code
4. React renders child component
5. `componentDidMount()` can cause side-effects but shouldn't update
the state (this would call render again and cause an infinite loop).

### Update lifecycle methods

When a component update is triggered by parents, the following lifecycle 
methods are called:

1. `componentWillReceiveProps(nextProps)` can sync state to props, but
shouldn't cause side-effects
2. `shouldComponentUpdate(nextProps, nextState)` may cancel updating process
if `false` is returned. It should not cause side-effects.
3. `componentWillUpdate(nextProps, nextState)` can sync state to props, but
shouldn't cause side-effects
4. `render()` prepares and structures JSX code
5. React update all children components props
6. `componentDidUpdate()` can cause side-effects but shouldn't update
the state (this would call render again and cause an infinite loop).

When a component update is triggred by internal change, the 
`componentWillReceiveProps` will not be called, but all following methods
will.

### PureComponent

We can use the `shouldComponentUpdate` method to decide whether a component
should be re-rendered based on `nextProps` and `nextState`

```jsx
shouldComponentUpdate(nextProps, nextState) {
  if (nextProps.title !== this.props.title) {
    return true;
  }
  
  return false;
}
```

Or we could implement a `PureComponent` that will always implement the
`shouldComponentUpdate` and check if `props` or `state` have changed.

```jsx
import React from 'react';

class Book extends React.Component {
  //...
}
```

As these can affect performances, we should only implement theme if
we know there's a possibilty of `props` on `state` not changing, thus
not needing React to re-render the component.

## Styling React component

To style a React component with scoped stypes, we can use 
[CSS Modules](https://github.com/css-modules/css-modules).

In a webpack config file:

```javascript
{
  loader: require.resolve('css-loader'),
  options: {
    importLoaders: 1,
    modules: true,
    localIdentName: '[name]__[local]__[hash:base64:5]'
  },
},
```

In a Book component:

```javascript
// ./Book/Book.js
import css from './Book.css';

class Book extends React.Component {
  render() {
    <div className={css.Book}>
      <h1 className={css.title}>Book title</h1>
    </div>
  }
}
```

```css
/* ./Book/Book.css */
.Book {
  margin: auto;
  width: 500px;
}

.title {
  color: red;
}
```

Webpack will affect hashes to the class names. This way, we can be sure our 
`title` class won't affect any other component than `Book`.

## Handling errors

Since React 16, we can handle errors in our React app using an ErrorBoundary
component:

```jsx
// ./ErrorBoundary/ErrorBoundary.js
import React from 'react';

export default class ErrorBoundary extends React.Component {
  state = {
    hasError: false,
    errorMessage: ''
  }

  componentDidCatch = (error, info) => {
    this.setState({
      hasError: true,
      errorMessage: error.message
    });
  }

  render() {
    if (this.state.hasError) {
      return (
        <div>
          <h1>Something went terribly wrong!</h1>
          <p>{this.state.errorMessage}</p>
        </div>
      );
    }

    return this.props.children;
  }
}
```

The `componentDidCatch` method act as the `catch` instruction and
will be triggered when an error is thrown in a child component. We
can now wrap the `ErrorBoundary` around the component from which we
want to catch errors.

```jsx
// App.js
// In a loop, the key props should be associated with the higher
// component
<ErrorBoundary key={movie.id}>
  <Book
    title={book.title}
    author={book.director}
  />
</ErrorBoundary>
```

If we `throw` in the `Book` component, the `ErrorBoundary` component
will be rendered instead of `Book` (in production only).


## Statefull vs stateless components

As an app grows, it becomes difficult to handle state management. A
good pratice is split apps into statefull (or container or class-based) 
components that change the state and stateless (or functional) components 
that only handles rendering based on passed props.

### Statefull (container) components

* Created with `class Book extends React.Component`
* Can access and update the state
* Can have lifecycle hook methods
* Access props and state via `this.props` and `this.state`
* Only used when we need to access state or lifecycle methods

### Stateless (functional) components

* Created with `const Book = (props) =>`
* Cannot access and update the state
* Cannot have lifecycle hook methods
* Only access props via `props`
* Use in every other case


## High-order component

High-order components are components that wraps another component to
add logic or elements that might be shared by multiple components. They
are often used by third-party librairies.

Create an HOC that add a wrapping `<div>` with a custom class:

```jsx
// ./hoc/WithClass.js
import React from 'react'

export default (props) => <div className={props.classes}>{props.children}</div>;
```

And use it in another component:

```jsx
// ./App.js
import WithClass from './hoc/WithClass'
//...
render() {
  return (
    <WithClass classes="App">
      <h1>Books</h1>
      <p>My bookshelf</p>
    </WithClass>
  );
}

```
An often seen pattern is to create a function that returns a wrapper element:

```jsx
// ./hoc/withClass.js
export default (WrappedComponent, className) => {
  return (props) => (
    <div className={className}>
      <WrappedComponent {...props}/>
    </div>
  );
}
```

That we would use like this:

```jsx
// ./App.js
import withClass from './hoc/withClass.js';
//...
render() {
  return (
    <React.Fragment>
      <h1>Books</h1>
      <p>My bookshelf</p>
    </React.Fragment>
  );
}
//...

export default withClass(App, '.App');
```
