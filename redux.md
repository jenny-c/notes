# Redux Notes 

## General Usage
- a state container for javascript apps 
  - the state of the app is stored in an object tree inside a single store 
  - the state is read-only, only mutable with an action 
  - transformed with pure reducers (always return new state objects, not mutate previous states)

- to change the state: emit an action (an object describing what happened)
  - reducers specify how the actions transform the state tree (take the state and an action as arguments) 

- API is subscribe (update UI in response to state changes), dispatch (dispatch actions to mutate the internal state), and getState (does what you think it does)
  - note: often redux is used with react so subscribe won't be used since there will be something else in the library 

- as things become more complex, it's good to split reducers up so that they each manage their own smaller part of the glabal state (they also each receive a different state parameter so it's even more modularized)
  - redux provides `combineReducers` which helps to simplify the code 

## Async
- asynchronous means there are two key moments: starting the call and receiving the answer (ex. isFetching, finishedFetching)
  - both moments typically require a change in the state 
- note: it's best for fetching data and stuff like that to be separate from other events early on so that it's easier to scale later on
- note: if there are nested entities that reference each other (ex. a post duplicated in multiple locations), they should be kept separately in the state as if it was a database 

## Middleware 
- middleware is the code put between the framework receiving a request and the one generating the response 
  - provides a third-party extension point between dispatching an action and when it reaches the reducer 
- useful applications:
  - logging
  - crash reporting

```javascript
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
    } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
} 

// applying the two functions above to a Redux store 
import { createStore, combineReducers, applyMiddleware } from 'redux'

const todoApp = combineReducers(reducers)
const store = createStore(
  todoApp,
  applyMiddleware(logger, crashReporter)
)
```

## React Redux 

### mapStateToProps
- used to select the part of the data that the connected component needs 
  - called every time the store state changes 
  - receives the entire store state, returns only the object of data the component needs 
  - defined as a function `function mapStateToProps(state, ownProps?)`
- the function essentially takes the store state and then maps the values to an object that the component needs 
  - note: it is possible for `mapStateToProps` to return a function which will then be used as the "final" `mapStateToProps`

- the purpose of this function is to "re-shape" the data from the store as needed for the component (ex. renaming, combining pieces of data, etc)
  - should be pure (**pure function**: a function which given the same input, will always return same output and has no side effects)

- shallow comparisons are done to see if results from the function have changed: if they have, the component will re-render
  - take care to not accidentally return new object or array references 
- **memoized selector**
  - **memoization:** an optimization techhnique used by storing results of expensive functions calls and returning the cached result when the same input occurs again
  - good to use memoized selectors for the necessary expensive operations when data changes 

#### Behaviour and Gotchas
- `mapStateToProps` will not run if the store state is the same (lastState === currentState)
  - `combineReducers` tries to optimize for this; if none of the slice reducers return a new value then `combineReducers` returns the old state object
- number of declared arguments affects behaviour 
  - the function runs whenever the root store state object is different, if `ownProps` is added too, it will also run when the wrapper props have changed 
  - so don't add `ownProps` unless actually necessary
```javascript
function mapStateToProps(state) {
  console.log(state) // state
  console.log(arguments[1]) // undefined
}
const mapStateToProps = (state, ownProps = {}) => {
  console.log(state) // state
  console.log(ownProps) // {}
}

function mapStateToProps(state, ownProps) {
  console.log(state) // state
  console.log(ownProps) // ownProps
}
function mapStateToProps() {
  console.log(arguments[0]) // state
  console.log(arguments[1]) // ownProps
}
function mapStateToProps() {
  console.log(args[0]) // state
  console.log(args[1]) // ownProps
}
```

### mapDispatchToProps
- used for dispatching actions to the store 

- there are two ways for components to dispatch actions: 
  - a connected component receives props.dispatch and can dispatch actions itself 
  - connect accepts an argument called `mapDispatchToProps` which lets you create functions that dispatch when called 

- `mapDispatchToProps` can be in the form of a function or as an object (function is more flexible but object is easier to use)
  - function: 
    - function will be called with dispatch as the first argument 
      - use by returning new functions that call dispatch() inside themselves
    - should return a plain object
    ```javascript
    const increment = () => ({ type: 'INCREMENT' })
    const decrement = () => ({ type: 'DECREMENT' })
    const reset = () => ({ type: 'RESET' })

    const mapDispatchToProps = dispatch => {
      return {
        increment: () => dispatch(increment()),
        decrement: () => dispatch(decrement()),
        reset: () => dispatch(reset())
      }
    }
    ```
    - these functions will be merged to the connected component as props and may be directly called there 
  - object:
      - preferred unless there is a specific reason to customize the dispatching behaviour 
      ```javascript 
      const mapDispatchToProps = {
        increment, 
        decrement, 
        reset
      }
      ```

