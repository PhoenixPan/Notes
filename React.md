## Component
1. Accept inputs (props) and return React element
2. function-based / class-based components
3. While this.props is set up by React itself and this.state has a special meaning, you are free to add additional fields to the class: e.g. `this.blur`
4. controlled component: changes only when the state changes


## State
JS object used to record to user events, each class has a state object. Whenever the component state is changed, the component rerenders, and also forces its children to re-render as well.
only class-based components have states, not function-based ones

1. Assign `this.state = {}` only in constructor, use this.setState()
https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous
2. Update partial state
    ```
    this.setState({
        [name]: value
        });
    ```
3. When to use state
    1. Is it passed in from a parent via props? If so, it probably isn’t state.
    2. Does it remain unchanged over time? If so, it probably isn’t state.
    3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

```
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

## Props
1. From parent component, read-only
2. All React components must act as pure functions with respect to their props.
    ```
    function withdraw(account, amount) {
    account.total -= amount;  // NOT ok in React
    }
    ```
3. Props are default to be `True`
4. `props.children` are special props for child elements between tag

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


