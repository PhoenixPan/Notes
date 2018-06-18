# Component
1. function-based / class-based components
2. You are free to add additional fields to the class: e.g. `this.blur`
3. controlled component: where React state is the "single source of truth"


# State
JS object used to record to user events, each class has a state object. Whenever the component state is changed, the component rerenders, and also forces its children to re-render as well.
only class-based components have states, not function-based ones

1. Use `this.setState()`, never mutate state directly (`state.push()`)
2. Don't rely on current state to calculate the next one: 
    ```
    this.setState({
        counter: this.state.counter + this.props.increment, // Wrong
    });
    ```
    [Reference](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)

3. Update partial state
    ```
    this.setState({
        [name]: value
        });
    ```

# Props
1. Read-only
2. All React components must act as PURE functions with respect to their props.
    ```
    function withdraw(account, amount) {
        account.total -= amount;  // NOT ok in React
    }
    ```
3. Props are default to be `True`
4. `props.children` are special props for child elements between tag

# Redux
Redux: Fetching data  
React: Displaying data

1. Container component: a normal React component managed by Redux state. When app state changes, it re-renders

## Action / Action Creator
1. Action objects always have "type"
2. Ways to add to the state:
    1. Through `Object.assign())`:
        ```
        Object.assign({}, state, {
            newState: value
        })
        ```
        Note: `Object.assign(state, { newState: value })` is wrong, it mutates the first argument.
    2. Spread operator:
        ```
        { ...state, ...newState }
        ```

## Reducer
Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API calls. No mutations. Just a calculation.

## redux-form
1. Flow:  
    1. Identify form states
    2. Make one Field component for each state
    3. User input
    4. RF handles changes 
    5. User submit
    6. RF validates and submits

2. Field status change: pristine->touched->invalid

## redux-promise
Redux-promise: is the action has a promise as payload? yes? stop the action, wait till it's resolved, and create a new action of the data, send it through

## react-router

1. `<BrowserRouter>`: interact with History lib 
2. `<Route>`: tell components based on URL

# Other
## JSX
1. Booleans, Null, and Undefined Are Ignored. This JSX only renders a <Header /> if showHeader is true: `{showHeader && <Header />}`

### React element:
Immutable: never change after creation, like a single frame of a movie




bind: how functions work in JavaScript. 
https://babeljs.io/docs/plugins/transform-class-properties/
https://reactjs.org/docs/handling-events.html#passing-arguments-to-event-handlers

The problem with arrow function binding is that a different callback is created each time the LoggingButton renders. In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.

typeof(arr) == "object" could be arr instanceof Array and it wouldn't try to loop over {Objects:''}

Check null for props, render() may perform before loading


