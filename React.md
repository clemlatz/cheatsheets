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
      {comments} // JSX knows how to render an array
    </div> );
  }
}
```

The `key` prop is a special prop that helps React keep track of which element
is which in the loop. It can help improve performance.

From within the component, arguments can be accessed with the *this.props*
object.

```javascript
class Books extends React.Component {
  render() {
    const topicsList = ['HTML', 'JavaScript', 'React'];
    return(
      <div className="book">
        <h3 className="book-title">{this.props.title}</h3>
        <p className="book-author">by {this.props.author}</p>
      </div>
    );
  }
}
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

    if (this.state.showComments) {
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
