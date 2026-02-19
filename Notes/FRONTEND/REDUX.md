- Its State Management library for JS apps.
- Redux is built for larger, more complex application.
- ==Redux toolkit== is the official recommendation of writing redux code.
- Its used for information passing props or sharing state. 
- Redux is central store which manages states.

## Setup:-
- Store
- Actions
- Reducer's


### Understanding Terms:-

- ***STORE*** : A centralized store holds the whole tree of your application.

- ***REDUCER*** : Functions that take the current state and an action as arguments, and return a new state result. in other words, 
	`(state, action)=> new state`.

- ***ACTION*** : It is plain JavaScript object that has type field.( like events).it must have a todo.

- ***SLICE*** : Collections of redux reducer logic and actions for a single feature together in a single file.

## Store:

- Here these are states where each can have all these states.
- We make a object which stores all states which all component can use.
- We cant change state directly using mutate in ==setState== . 
- Redux also has function which is use to mutate which are called ==Reducers==
- We tell reducer which state is changes and what action to perform.`[State,action]`

![[Pasted image 20260209154627.png]]

### Redux Toolkit:-
`npm install @reduxjs/toolkit`
`npm install react-redux`


- The **Redux Toolkit** package is intended to be the standard way to write [Redux](https://redux.js.org/) logic. It was originally created to help address three common concerns about Redux:

	- "Configuring a Redux store is too complicated"
	- "I have to add a lot of packages to get Redux to do anything useful"
	- "Redux requires too much boilerplate code"

## Creating reducer:-

- Redux toolkit automatically generates action creators(function that creates action  objects).
- **(state, action)=>{//update state}**

 *Redux toolkit lets you write simpler immutable update logic using "mutating" syntax.*

## Provider Component:-
- the `<provider>` component makes the redux store available to any nested components that need to access the redux store.
```jsx
import Todo from "./components/todo"
import { Provider } from "react-redux"
import { store } from "./app/store"
import './App.css'

  

function App() {

  return (
    <>
    <Provider store={store}>
      <Todo/>
    </Provider>
    </>
  )
}

export default App
```

*To use store or state variable in the other files*

## Dispatching action:-
- **Tringgering a state change

#### useSelector:-
- The useSelector hooks allows you to extract data or state from the redux store useState selector function.(returns the data).

#### useDispatch:- 
- The useDispatch hook allows to send or dispatch an action to the redux store by giving the action as an argument to dispatch variable.