# Redux

Redux aims at solving a web development problem: our code must manage more than ever before. With modern front-end frameworks, the difficulty increases because we're mixing two concepts: mutation and asynchronicity. Redux attemps to make state mutations predictable by imposing certains restrictions on how and when updates can happen.

These restrictions are reflected in the three principles of Redux:
- *Single source of truth*: the state of the whole application is store in an object tree within a single store.
- *State is read-only*: the only way to change the state is to emit an action, an object describing what happened.
- *Changes are made with pure functions*: to specify how the state tree is transformed by actions, we write pure reducers.


## Actions

Actions are payloads of information that send data from the application to the store. They are the *only* source of information for the store. They are sent to the store using `store.dispatch()`.

```javascript
{
    type: 'ADD_MOVIE',
    title: 'Isle of dogs'
}
```

- Actions are plain JavaScript objects.
- They must have a `type` property that indicates the type of the action being performed.
- Other than `type`, the structure of an action can be freely defined. See Flux Standard Action for a recommendation on how actions could be constructed.

A best practice is to defined types as string constants. Large apps can store them into a separate module.

```javascript
import { ADD_MOVIE, REMOVE_MOVIE } from '../actionTypes';
```

```javascript
{
    type: ADD_MOVIE,
    title: 'Three Billboards Outside Ebbing, Missouri'
}
```


### Actions Creators

Action creators are function that create actions. They simply return an action, that can then be passed to the `dispatch` function.

```javascript
function addMovie(title) {
    return {
        type: ADD_MOVIE,
        title
    }
}

dispatch(addMovie('Darkest Hour'));
```

Alternatively, it is possible to create a bound action creator that automatically dispatches.

```javascript
const boundAddMovie = title => dispatch(addMovie(title));

boundAddMovie('Ready Player One');
```
