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

or using the shorthand `<>`:

```jsx
render() {
  return (
    <>
      <h1>Books</h1>
      <p>My bookshelf</p>
    </>
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

### Validating props with PropTypes

We can validate props and ensure passed props are of the correct types using
the [PropTypes library](https://github.com/facebook/prop-types).

    npm install --save prop-types
    
In the component:

```jsx
import React from 'react';
import PropTypes from 'prop-types';

class Book extends React.Component {
  render() {
    return (
      <div className="book">
        <h1>{this.props.title} ({this.props.year})</h1>
        <p>by {this.props.author}</p>
        <button onClick={this.props.onDeleteButtonClick}>delete</button>
      </div>
    );
  }
}

Book.propTypes = {
  title: PropTypes.string.isRequired,
  author: PropTypes.string.isRequired,
  year: PropTypes.number.isRequired,
  onDeleteButtonClick: PropTypes.func.isRequired
};

export default Book;
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

## Hooks

Hooks are special functions that let us "hook" in to React feature, like state,
in functional components.

### Hooks rules

Those rules can be enforced with `eslint-plugin-react-hooks`.

- Hooks should always be used in a component at top level, so that it is 
guaranteed they are called in the same order every time.
- Hooks should always be called from React functions, ie functionnal components
or another hook

### useState

The `useState` hook allows to get and update state. It returns an array 
containing a getter and a setter for a state property.

```js
const [value, setValue] = useState(initialValue);
```

A functional component using the `useState` hook:

```jsx
function CounterButton() {
  const [counter, setCounter] = useState(0);
  return (
    <button onClick={() => setCounter(counter + 1)}>
      {counter}
    </button>
  );
}
```

### useRef

The `useRef` hook allow us to directly access an element in the DOM. It returns
an object that can be associated to a DOM element using it's `ref` attribute,
and whose `current` property is a pointer to that element. 

```js
const ref = useRef(null);
return <div><img src="/image.jpg" ref={ref}></div>
```

A functional component using the `useRef` hook to change an image on mouse over:

```jsx
function SwapImage() {
  const imageRef = useRef(null);
  const imageNB = '/images/image-nb.jpg';
  const imageColor = '/images/image-color.jpg';

  return (
    <img 
      src={imageNB} 
      onMouseOver={() => {
        imageRef.current.src = imageColor;
      }}
      onMouseOut={() => {
        imageRef.current.src = imageNB;
      }}
      alt="swappable image!" 
    />
  );
}
```

### useEffect

The `useEffect` hooks allows us to us side effects in a React functionnal
components, and execute functions after the component has been rendered or
unmounted. 
As an example, adding and removing DOM listeners is a use case for this hook. 

```jsx
useEffect(() => {
  // This is executed when the component renders
  return () => {
    // This is executed when the component unmounts
  }
}, [dependency]);
```

The second argument is an array that lists dependencies for the hooks. 
- If left out, the hook will be executed on first render and at every subsequent 
  rerender of the component.
- If empty, the hook will only be executed when the component is first mounted.
- If the array includes an item, the hook will be executed only if the value of
  the value has changed since previous execution

```jsx
const [checkboxValue, setCheckboxValue] = useState(false);
useEffect(
  () => console.log('checkboxValue changed!'),
  [checkboxValue]
);
```

A functional component using the `useEffect` hook to change an image if it's
completely in view:

```jsx
function ImageToggleOnScroll({ imageInView, imageNotInView }) {
  const imageRef = useRef(null);
  const [inView, setInView] = useState(false);

  const isInView = () => {
    const rect = imageRef.current.getBoundingClientRect();
    return rect.top >= 0 && rect.bottom <= window.innerHeight;
  };

  const scrollHandler = () => {
    setInView(isInView());
  };

  useEffect(() => {
    window.addEventListener("scroll", scrollHandler);
    return () => {
      window.removeEventListener("scroll", scrollHandler);
    };
  }, []);

  return (
    <img
      src={inView ? imageInView : imageNotInView}
      alt="A random pic"
      ref={imageRef}
    />
  );
}
```

### useContext

The `useContext` allows to access context in a much more simpler way than the
traditionnal API.

```jsx
// App.jsx
// Create context for the entire app
import React from 'react';
export const ConfigContext = React.CreateContext();

// This is the value we want to access in any component below this one
const = config = {
  showBookCovers: true;
}

const App = () => {
  return (
    {/* Wrap our component in the context provider and assign it a value */}
    <ConfigContext.Provider value={config}>
      <div className="App">
        <Books />
      </div>
    </ConfigContext.Provider>
  );
};
```

We can access the context from within another component below `App`:

```jsx
// Books.jsx
import React, { useContext } from 'react';
import { ConfigContext } from './App';

const Book = ({ title, coverImage }) => {

  const config = useContext(ConfigContext);

  return (
    <div className="Book">
      {config.showBookCovers ? <img src={coverImage} /> : null}
      {title}
    </div>
  );
};
```

### useReducer

A reducer is a function that takes a previous state as first parameter, an 
action to update state as a second parameter, and returns a new state.

```js
(previouState, action) => newState
```

A `useState` hook can be replaced with a `useReducer` hooks with the same
functionnality.

```js
const [value, setValue] = useState(initialValue);
setValue('new value');

// becomes

const [value, dispatchValue] = useReducer((state, action) => {
  switch (action.type) {
    case 'setValue':
      return action.data;
    default:
      return state;
  }
  action
}, initialValue);
dispatchValue({
  type: 'setValue',
  data: 'new value';
})
```

A functional components that uses a reducer to update a book list.

```jsx
import React, { useReducer } from 'react';

const BookList = () => {
  const [books, dispatchBooks] = useReducer(
    (state, action) => {
      switch (action.type) {
        case 'addBook':
          return state.push(action.book);
        case 'deleteBook':
          const bookIndex = state.findIndex(book => book.id === action.bookId);
          return state.splice(bookIndex, 1);
        default:
          return state;
      }
    }, 
    []
  );

  function deleteBook(bookId) {
    dispatchBooks({ action: 'deleteBook', bookId });
  }

  return (
    <div className="BookList">
      {books.map(
        book => <Book book={book} onClick={() => deleteBook(book.id)} />
      )}
    </div>
  );
};
```

One use-case for `useReducer` is to validate 
and/or update multiple state values.

```js
// `state` will contain the current value while
// `action` will be the first argument passed to the `setEmail` function
const emailReducer = (state, action) => {
  const isEmailValid = validateEmail(action);
  setEmailValid(isEmailValid);
  return isEmailValid ? action : state;
}
const initialValue = '';
const [email, setEmail] = useReducer(emailReducer, initialValue);
//...
setEmail('user@example.org');
```


### useCallback & useMemo

The `useCallback` and `useMemo` hooks are used for Memoization (an optimization
technique used for returning cached results).
- `useMemo` caches a value
- `useCallback` caches a function

If we have some calculation done when the component renders (eg. filtering and
sorting results), we don't want that calculating to be done at every renders,
but maybe only when some props changes (eg filter criteria or sort order).


```jsx
import React, { useMemo } from 'react';

const BookList = ({ books }) => {
  const [listFilter, setListFilter] = useState('');
  const [listOrder, setListOrder] = useState('title');

  const filteredBooks = books.filter(book => book.title.includes(listFilter));
  const sortedBooks = sortByProperty(filteredBooks, listOrder);

  return sortedBooks.map(book => <Book book={book} />);
};
```

In the example above, books will be filtered and sorted at every `BookList` 
render, even if `books`, `listFilter` and `listOrder` hasn't changed. When
can use the `useMemo` hook to fix that.

```jsx
import React, { useMemo } from 'react';

const BookList = ({ books }) => {
  const [listFilter, setListFilter] = useState('');
  const [listOrder, setListOrder] = useState('title');

  const sortedBooks = useMemo(
    () => sortByProperty(
      books.filter(
        (book) => book.title.includes(listFilter), 
        listOrder
      )
    ),
    [books, listFilter, listOrder]
  );

  return sortedBooks.map(book => <Book book={book} />);
};
```

The first argument of `useMemo` is a function that returns the value we want to
memoize, the second argument is a list of dependencies that, when changed, will
invalidate the cached value and calculate the result again.

