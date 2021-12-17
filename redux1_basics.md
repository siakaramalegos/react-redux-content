---
theme: style.css
verticalSeparator: -v-
highlightTheme: github
---

# Redux Basics
## Sia Karamalegos

-v-

# Redux

Redux is a predictable state container for JavaScript apps.

Note: Web applications have become more complex over time. As developers, we must manage so much more in state than before, including temporary form data, server or API responses for persisted data, and cached data... which route we are currently on as well as the browser history of routes. If you add UX state on top of that - loading data spinners, pagination, modal active/inactive state, tab selections, etc. - you can see how state can quickly become unmanageable and bug-prone. Not only do we have to manage real-time updates to state but asynchronous ones as well! How do you fix it when something goes wrong?!?!

-v-

## Redux helps write apps that...

- Behave consistently,
- Run in different environments,
- Are easy to test.

Note: enviroments = (client, server, and native). You can use Redux together with React, or with any other view library. It is tiny (2kB, including dependencies).

-v-

## Redux alternatives

- Local state (no package)
- MobX, MobX-State Tree
- Unstated
- Apollo-link-state
- Other stuff

-v-

## What are we learning about?

### Redux Basics

- Primary principles
- Actions and reducers
- The store and dispatching
- Pure functions
- Action creators and constants
- More on reducers
- Subscribing to the store

---

# Primary principles

-v-

## I. Global State

One of the primary principles of Redux is that there should be one store, or state, for the entire application that serves as the single source of truth.

Usually, this state is represented as a plain object, but it can be any data type.

-v-

```javascript
{
  puppies: [
    {
      name: 'Olivia Newton John',
      breed: 'Australian Shepherd',
      available: true,
    },
    {
      name: 'Evander Holyfield',
      breed: 'Boxer',
      available: false,
    },
  ],
  filter: 'SHOW_AVAILABLE',
}
```

-v-

## II. Data always flows one-way

To update the state:

1. Dispatch an action from the store.
2. Maybe dispatch thunks or other actions from that action.
3. The reducer listens for the actions and returns a new state to the store.

Note: Redux Thunk middleware allows you to write action creators that return a function instead of an action. The thunk can be used to delay the dispatch of an action, or to dispatch only if a certain condition is met. The inner function receives the store methods dispatch and getState as parameters.

-v-

<img style="border:0;background:inherit;box-shadow:none;" src="https://s3.amazonaws.com/viking_education/web_development/web_app_eng/redux_dispatch.png" alt="Dispatching in Redux" width="80%">

Note: Getting a solid foundation in the core process and patterns of Redux is critical to understanding BEFORE we incorporate it into our React applications. We have a single source of truth in our 1 store that holds our application state. From our store, we dispatch action objects that describe the event and pass any data required. Then our reducers listen for the action and send a new state to the store using pure functions.

---

# Actions and Reducers

<img src="https://s3.amazonaws.com/viking_education/web_development/web_app_eng/greedy_pup.gif" alt="Greedy puppy stealing other dogs' treats animated gif">

-v-

## Actions

Actions are **objects** that describe the event that is happening.

- Sometimes send data to the reducer, and thus the store
- The only data source to send data to the store
- Always have a `type`
- Sometimes have `data` or `payload`, `error`, and/or `meta`

Note: Flux Standard Actions

-v-

## Action Examples

```javascript
// Successful add todo action
{
  type: 'ADD_TODO',
  payload: {
    text: 'Do something.'
  }
}

// Failed add todo action which instead threw an error
{
  type: 'ADD_TODO',
  payload: new Error(),
  error: true
}
```

-v-

## Reducers

- Reducers are **pure functions** which take an action and update the state into the next state.
- Why are they called "reducers"? The name comes from [`Array.prototype.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

<img src="https://s3.amazonaws.com/viking_education/web_development/web_app_eng/dog_shrinking.gif" alt="Dog shrinking machine animated gif">

Note: which "applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value". If you think about a reducer in Redux, it is returning the accumulation of the state based on all the previous actions. Each action is analogous to one cycle of the `Array.prototype.reduce()` function.

-v-

## Reducer Example

```javascript
// Arithmetic reducer
function numberReducer(state = {myNumber: 0}, action) {
  switch (action.type) {
    case 'ADD':
      return {
        ...state,
        myNumber: state.myNumber + action.data
      }
    default:
      return state
  }
}
```

Note: walk through this - 2 args, state and action - then switch/case for each action option...

-v-

## Hello, Redux!

1. `npx create-react-app redux_puppies`
2. `cd redux_puppies`
3. `npm start` or `yarn start`
4. In `src/`, create `reducers.js`
5. Create the puppies reducer with an initial state of `[]` which handles a `CREATE_PUPPY` action

```javascript
function myReducer(state, action) {
  switch (action.type) {
    case 'MY_ACTION':
      return {
        ...state,
        myStateProperty: action.data
      }
    default:
      return state
  }
}
```

Note: Don't forget the default cause. Why did I add the puppies the way I did? Pure functions...

---

# The store and dispatching

-v-

## The Store

- The **store** holds the state and allows us to access it and dispatch actions.
- Generally, there is only one store in a Redux application.
- Use `getState()` to see the current state.

```javascript
// Create the store
const store = Redux.createStore(puppies)

// Access the state from the store
console.log('store state', store.getState())
```

Note: this is the first time we've needed the Redux library thus far.

-v-

## Dispatching Actions

`dispatch()` dispatches actions to the reducer. It accepts one argument - the action object.

```javascript
// Dispatch an action
store.dispatch({
  type: 'CREATE_PUPPY',
  data: {
    name: 'Priscilla Queen of the Desert',
    breed: 'Australian Shepherd',
  }
})

// See the new state
console.log('new state', store.getState())
```

-v-

## You Do:

1. Add Redux `yarn add redux`
2. In `src/index.js`, create the store
2. Dispatch `CREATE_PUPPY` actions
3. Verify that the store received the changes with `console.log()`

```javascript
// Create the store
const store = Redux.createStore(reducer)

// Access the state from the store
store.getState()

// Dispatch an action
store.dispatch(action)
```

Note: What else will we need here? `createStore` and the `puppies` reducer. What was a pain here? Matching up the action to what we said in our reducer. So now we have 2 things to follow up on: pure functions and action creators.

---

# Pure Functions

<img src="https://s3.amazonaws.com/viking_education/web_development/web_app_eng/puppy_detour.gif" alt="Funny puppy taking a detour off a porch into a bush gif">

Note: You are probably familiar with object-oriented programming in JavaScript or Ruby. Since Redux relies heavily on some functional programming concepts, we will take a quick detour to familiarize ourselves with them.

-v-

## Pure Functions

**Pure functions** always return the same output if given the same inputs, and have no side effects.

- Non-primitive objects in JavaScript are only passed as **references** into functions.
- If you change a property of an object inside a function, it will change outside of the function as well which is a **side effect**!

Note: Primitives = strings, numbers, and booleans

-v-

### Impure Function Example

```javascript
const myPuppy = {
  name: "Harry",
  age: 4,
  chewedItems: ['rug', 'sheets'],
}

// Impure puppy updater function
function updatePuppy(puppy, newName) {
  puppy.name = newName
  return puppy
}

const firstPuppy = updatePuppy(myPuppy, 'Chewy')
const secondPuppy = updatePuppy(myPuppy, 'Bitey')

console.log(secondPuppy.name)
// => Bitey

console.log(firstPuppy.name)
// => Bitey
// Changes previous puppy. Oops!
```

Note: what's the name of the second puppy? Bitey. What's the name of the first puppy? Oops.

-v-

### Refactor to Pure Function

[`Object.assign(target, ...sources)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

```javascript
// Pure puppy updater using Object.assign()
function pureUpdatePuppy(puppy, newName) {
  return Object.assign({}, puppy, {name: newName})
}

const thirdPuppy = pureUpdatePuppy(myPuppy, 'Sir-Chew-A-Lot')
console.log(thirdPuppy.name)
// => Sir-Chew-A-Lot

console.log(secondPuppy.name)
// => Bitey
// Doesn't change previous puppy! Woot!
```

Note: The Object.assign() method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.

-v-

### Refactor to Pure Function

[Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

```javascript
// Pure puppy updater using spread
function pureUpdatePuppySpread(puppy, newName) {
  return {
    ...puppy,
    name: newName,
  }
}

const fourthPuppy = pureUpdatePuppy(myPuppy, 'Mike Tyson')
console.log(fourthPuppy.name)
// => Mike Tyson

console.log(secondPuppy.name)
// => Bitey
// Doesn't change previous puppy! Woot!
```

Note: Spread syntax allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected. The spread operator is only available in ES2015, so for this code to work, you will need Babel. Conveniently, Babel offers a [repl here](https://babeljs.io/repl/).

-v-

### Arrays

`pop`, `push`, and `shift` mutate arrays rather than returning new ones.

```javascript
// Impure puppy chewedItems updater
function addChewedItem(puppy, newItem) {
  puppy.chewedItems.push(newItem)
  return puppy
}

const fifthPuppy = addChewedItem(myPuppy, 'headphones')
console.log(fifthPuppy.chewedItems)
// => ["rug", "sheets", "headphones"]

const sixPuppy = addChewedItem(myPuppy, 'turntable')
console.log(sixPuppy.chewedItems)
// => ["rug", "sheets", "headphones", "turntable"]

console.log(fifthPuppy.chewedItems)
// => ["rug", "sheets", "headphones", "turntable"]
// Changes previous puppy. Oops!
```

-v-

### Arrays

Use `Object.assign()` or the spread operator to return a new array. `Array.prototype.map()` also returns a new object.

```javascript
// Pure add chewed item
function pureAddChewedItem(puppy, newItem) {
  return {
    ...puppy,
    chewedItems: [
      ...puppy.chewedItems,
      newItem,
    ],
  }
}

const seventhPuppy = pureAddChewedItem(myPuppy, 'Priscilla')
const eigthPuppy = pureAddChewedItem(myPuppy, 'speaker')
console.log(eigthPuppy.chewedItems)
// => ["rug", "sheets", "speaker"]

console.log(seventhPuppy.chewedItems)
// => ["rug", "sheets", "Priscilla"]
// Doesn't change previous puppy! Woot!
```

-v-

### Why Is This Important?

- Without pure functions, multiple asynchronous actions dispatched close together could cause a a [race condition](https://en.wikipedia.org/wiki/Race_condition).
- With pure functions, each request, or function, is completely independent of the other. Order no longer matters.
- Thus, pure functions can greatly simplify our code and make debugging easier.

---

# Action Creators and Constants

-v-

## Action Creators

Functions that return actions for easy reusability:

```javascript
// Dispatching regular action to create puppy
store.dispatch({
  type: 'CREATE_PUPPY',
  data: {
    name: 'Priscilla Queen of the Desert',
    breed: 'Australian Shepherd',
  }
})

// Action creator to create a puppy
function createPuppy(data) {
  return { type: 'CREATE_PUPPY', data }
}

// Dispatching with an action creator
store.dispatch(createPuppy({
  name: 'Priscilla Queen of the Desert',
  breed: 'Australian Shepherd',
}))
```

Note: The benefit is that now instead of re-typing the full object with the type property, we can just send the data needed for the puppy. In addition, they let us pull additional logic out of components, which is easier to maintain and change in applications under heavy development. We will see this later when we learn async actions.

-v-

## Action Types as Constants

In larger projects, define the action **types** as constants for naming consistency and quicker error catching.

```javascript
// Set constants for action types
const CREATE_PUPPY = 'CREATE_PUPPY'
const ADOPT_PUPPY = 'ADOPT_PUPPY'
const UPDATE_PUPPY = 'UPDATE_PUPPY'

// Action creators for each action
function adoptPuppy(id) {
  return {
    type: ADOPT_PUPPY,
    data: id,
  }
}
```

Note: You then export those constants for reuse in your reducers. Otherwise, you can sometimes fail silently if you type an action type.

-v-

## You Do

1. Create action constants and action types in a new `src/actions.js` file.
2. Reuse the constants in your puppies reducer.
3. Refactor your dispatches to use action creators instead.
4. If you have time, add the adopt and update puppy actions.

```javascript
// Set constants for action types
export const MY_ACTION = 'MY_ACTION'

// Action creators for each action
export function myAction(data) {
  return { type: MY_ACTION, data }
}
```

Note: We'll need to export and import a lot of stuff now.

---

# More on Reducers

-v-

## A Note on Designing State Shape

- Before writing reducers, design the initial state and its "shape" for your application.
- Use the simplest, or minimal, design for your state that accommodates your app features.

```javascript
{
  availabilityFilter: 'SHOW_ALL',
  puppies: [
    {
      id: 1,
      name: 'Sean Connery',
      breed: 'Scottish Terrier',
      available: true,
    },
    {
      id: 2,
      name: 'Priscilla Queen of the Desert',
      breed: 'Australian Shepherd',
      available: true,
    },
  ]
}
```

Note: How would you add your availability filter to your reducer(s)? You could put it in the same reducer, or you could create a second reducer...

-v-

## One Giant Reducer

```javascript
// Reducer function using default initial state
function puppyAdoptionApp(state = initialState, action) {
  switch (action.type) {
    // Use the same constants set in the actions for type.
    case SET_AVAILABILITY_FILTER:
      return {
        ...state,
        availabilityFilter: action.data,
      }
    case CREATE_PUPPY:
      return {
        ...state,
        puppies: [
          ...state.puppies,
          action.data
        ]
      }
    // More puppy actions
    default:
      return state
  }
}
```

-v-

## Splitting Unwieldy Reducers

```javascript
// Puppies reducer - initial state is simply an empty array
function puppies(state = [], action) {
  switch (action.type) {
    case CREATE_PUPPY:
      return [
        ...state,
        action.data
      ]
    //...
    default:
      return state
  }
}

// Availability filter in more generic app reducer
// Initial state is just our default filter
function availabilityFilter(state = SHOW_ALL, action) {
  switch(action.type) {
    case SET_AVAILABILITY_FILTER:
      return action.data
    default:
      return state
  }
}
```

-v-

## Combining Reducers

```javascript
// Write your own
function puppiesApp(state = {}, action) {
  return {
    puppies: puppies(state.puppies, action)
    availabilityFilter: availabilityFilter(
      state.availabilityFilter,
      action,
    ),
  }
}

// Or use combineReducers helper function
import { combineReducers } from 'redux'

const puppiesApp = combineReducers({
  // Use the reducer function names
  puppies,
  availabilityFilter
})
```

-v-

## You Do:

1. Create an availability filter in a separate reducer.
2. Use `combineReducers` to refactor your store.
3. Verify that it works.

```javascript
import { combineReducers, createStore } from 'redux'

const myReducers = combineReducers({
  // Use the reducer function names
  reducer1,
  reducer2
})

const store = createStore(myReducers)
```

Note: Notice how the state shape changes. Why is it logging every action twice?

---

# Subscribing to the Store

-v-

## Subscribing to State Updates

- The `subscribe()` function listens for any time an action is dispatched and part of the state tree may have changed.
- It returns another function that unregisters the listener.

```javascript
// Set up the listener while also declaring the unregister function
// so that we can access it to unsubscribe later.
const unsubscribe = store.subscribe(() => {
  // Log every state change to the console
  console.log(store.getState())
})

// Dispatches...

// Unregister/cancel the listener
unsubscribe()
```

Note: Since we don't have our application hooked into a front-end like React yet, it would be nice to see our updates to the store. Conveniently, Redux provides the `subscribe()` function that listens for any time an action is dispatched. The `subscribe()` function returns another function that will unregister the listener. If we wrap our dispatch calls in a subscribe and unsubscribe, we can set up logging to the console without having to manually log after each dispatch.

-v-

## You Do:

Refactor your application to subscribe to the store and remove your `console.log` statements.

```javascript
// Set up the listener while also declaring the unregister function
// so that we can access it to unsubscribe later.
const unsubscribe = store.subscribe(() => {
  // Log every state change to the console
  console.log(store.getState())
})

// Dispatches...

// Unregister/cancel the listener
unsubscribe()
```

---

# Wrapping Up

-v-

## Actions and Reducers

At their core, **ACTIONS** are just objects that describe an event that is happening.

**REDUCERS**  take that action and the previous state and return a new state.

The main trick with reducers is to make sure you always use **PURE FUNCTIONS**.

Note: Now that we have our actions and reducers ready to go, it's time to create our store and dispatch some actions.

-v-

## Data always flows one-way

<img style="border:0;background:inherit;box-shadow:none;" src="https://s3.amazonaws.com/viking_education/web_development/web_app_eng/redux_dispatch.png" alt="Dispatching in Redux" width="80%">

Note: Getting a solid foundation in the core process and patterns of Redux is critical to understanding BEFORE we incorporate it into our React applications. We have a single source of truth in our 1 store that holds our application state. From our store, we dispatch action objects that describe the event and pass any data required. Then our reducers listen for the action and send a new state to the store using pure functions.
