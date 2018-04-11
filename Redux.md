# Redux

Redux aims at solving a web development problem: our code must manage more than ever before. With modern front-end frameworks, the difficulty increases because we're mixing two concepts: mutation and asynchronicity. Redux attemps to make state mutations predictable by imposing certains restrictions on how and when updates can happen.

These restrictions are reflected in the three principles of Redux:
- *Single source of truth*: the state of the whole application is store in an object tree within a single store.
- *State is read-only*: the only way to change the state is to emit an action, an object describing what happened.
- *Changes are made with pure functions*: to specify how the state tree is transformed by actions, we write pure reducers.