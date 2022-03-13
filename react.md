# React

React is a JavaScript library used for user interfaces creation in Single Page Applications.
React uses object oriented and functional programming, with more focus on the latter.
React is declarative, which make your code more predictable and easier to debug.
React is component based, which are pieces encapsulated of code that manage tehir own state.

# JSX

JSX stands for JavaScript XML. JSX allows us to write HTML in React. JSX makes it easier to write and add HTML in React.
JSX produces React “elements”. And it is syntactic shugar for [React.createElement](https://reactjs.org/docs/react-api.html#createelement)

```js
<a className="link" src="google.com">
    <span>Link<span>
</a>

// Behind the scenes the above will be transformed in

React.createElement(
    'a',
    {
        src: 'google.com',
        className: 'link',
    },
    [
        React.createElement(
            'span',
            {},
            'Link'
        )
    ]
)
```

You can put any valid JavaScript expression inside the curly braces in JSX (like in template literals).

```js
const user = {
    firstName: 'Harper',
    lastName: 'Perez'
};

<h1>
    Hello, {user.firstName + ' ' + user.lastName}!
</h1>
```

JSX is an expression too, which means you can use it inside `if` statements.
Store it into `variables`, use it in `for` loops, and so on.

```js
let element = <p>This is a p stored in a variable</p>

if(someCondition) {
    element = <h1> Now this is a title stored in a variable <h1>
}
```

Since JSX is closer to JavaScript than to HTML, React DOM uses camelCase property naming convention instead of HTML attribute names.
For example, class becomes className in JSX, and tabindex becomes tabIndex.

# Componnets

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

The simplest way to define a component is to write a JavaScript function (`functional component`):

```js
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}
```

You can also use an ES6 class to define a component (`class componnet`):

```js
class Welcome extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

The above two components are equivalent from React’s point of view.

# Props

HTML tags have attributes (for example class, style, src) which, we can say, configure a tag behavior somehow. React components do not have attributes. However, components have properties (other name: props) which play the same role as HTML attributes for HTML tags. Props are received from "the above" (usually directly from the parent) and must not be overwritten (thus, we can say that they are immutable if they come from "the above").

### `Props are read-only`

Whether you declare a component as a function or a class, it must never modify its own props.

Consider this sum function:

```js
function sum(a, b) {
    return a + b;
}
```

Such functions are called “pure” because they do not attempt to change their inputs, and always return the same result for the same inputs.

`All React components must act like pure functions with respect to their props.`

# State

Sometimes we need to store some data inside a component. If the data is important only for that particular component and can change over time because of different triggers (for example user interaction, websocket messages or some internal changes in application), we use state.

`There are three things you should know about setState().`

1. `Do Not Modify State Directly`

```js
// Wrong
this.state.comment = 'Hello';
// Correct
this.setState({comment: 'Hello'});

// The only place where you can assign this.state is the constructor.
this.state = { comment: 'Hello' };
```

2. `State Updates May Be Asynchronous`

React may batch multiple setState() calls into a single update for performance.

Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.

If current state depends on previous state use a second form of setState() that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

```js
// Wrong
this.setState({
    counter: this.state.counter + this.props.increment,
});

// Correct
this.setState((state, props) => ({
    counter: state.counter + props.increment
}));
```

3. `State Updates are Merged`

```js
constructor(props) {
    super(props);
    this.state = {
        posts: [],
        comments: []
    };
}

// Then you can update them independently with separate setState() calls:

this.setState({
    posts: response.posts
});

this.setState({
    comments: response.comments
});

// The merging is shallow, so this.setState({comments}) leaves this.state.posts intact, but completely replaces this.state.comments.
```

### The Data Flows Down

Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.

This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

A component may choose to pass its state down as props to its child components:

```js
<FormattedDate date={this.state.date} />
```

The FormattedDate component would receive the date in its props and wouldn’t know whether it came from the Clock’s state, from the Clock’s props, or was typed by hand:

```js
const FormattedDate = (props) => {
    return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.

If you imagine a component tree as a waterfall of props, each component’s state is like an additional water source that joins it at an arbitrary point but also flows down.

In React apps, whether a component is stateful or stateless is considered an implementation detail of the component that may change over time. You can use stateless components inside stateful components, and vice versa.

# Difference between props and state

Props and state have much different roles in a component. We can summarize the difference between component's props and state like this:

1. Can get initial value from parent Component?
    - props: `yes`
    - state: `yes`
1. Can be changed by parent Component?
    - props: `yes`
    - state: `no`
1. Can set default values inside Component?
    - props: `yes`
    - state: `yes`
1. Can change inside Component?
    - props: `no`
    - state: `yes`
1. Can set initial value for child Components?
    - props: `yes`
    - state: `yes`
1. Can change in child Components?
    - props: `yes`
    - state: `no`

# Conditional rendering

There are 3 ways we can choose to render or not a component based on a given condition:

```js
const UsingAnIf = (props) => {
    let conditionalRenderedElement = null

    if(props.condition) {
        conditionalRenderedElement = (
            <h1>Hello World!</h1>
        );
    }

    return (
        <div>
            {conditionalRenderedElement}
        </div>
    )
}
```

```js
const UsingTernaryOperator = (props) => {
    return (
        <div>
            {
                props.condition ? (
                    <h1>Hello World!</h1>
                ) : null
            }
        </div>
    )
}
```

```js
const UsingSHortCircuitSyntax = (props) => {
    return (
        <div>
            {
                props.condition && (
                    <h1>Hello World!</h1>
                )
            }
        </div>
    )
}
```

### Preventing Component from Rendering

In rare cases you might want a component to hide itself even though it was rendered by another component. To do this return null instead of its render output.

```js
// If props.warn is falsy this component will not render
function WarningBanner(props) {
    if (!props.warn) {
        return null;
    }

    return (
        <div className="warning">
        Warning!
        </div>
    );
}
```

# Events

Handling events with React elements is very similar to handling events on DOM elements.

```js
const ButtonFunctionalComponent = () => {
    const onClickHandler = () => {
        console.log('Button inside ButtonFunctionalComponent was clicked !');
    }
    return (
        <button onClick={onClickHandler}>
            Click Me!
        </button>
    )
}
```

```js
class ButtonClassComponent {
    constructor() {
        super();
    }

    onClickHandler() {
        console.log('Button inside ButtonClassComponent was clicked !');
    }

    render() {
        return (
            <button onClick={this.onClickHandler}>
                Activate Lasers
            </button>
        )
    }
}
```

When you define a component using an ES6 class, a common pattern is for an event handler to be a method on the class. For example, this Toggle component renders a button that lets the user toggle between “ON” and “OFF” states:

```js
class Toggle extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isToggleOn: true
        };

        // This binding is necessary to make `this` work in the callback
        this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
        // Here we use setState second form because current state
        // depends on the value of previous state
        this.setState((prevState) => ({
            isToggleOn: !prevState.isToggleOn
        }));
    }

    render() {
        return (
            <button onClick = {this.handleClick}> 
                {this.state.isToggleOn ? 'ON' : 'OFF'}
            </button>
        );
    }
}
```

If you do not want to bind the event handler, you can use an arrow function:

```js
render() {
        return (
            <button onClick = {() => this.handleClick()}> 
                {this.state.isToggleOn ? 'ON' : 'OFF'}
            </button>
        );
    }
```

### Passing an event to a React Component

Lets say that I have a component which has a button and I want to know from above (Parent Component) when the button inside the Child Component was clicked?

```js
const ChildComponnet = (props) => {
    const onClickHandler = () => {
        console.log('Hello From Child Component!');
        props.onClickFromParentComponnet();
    }
    return (
        <button onClick={onClickHandler}>
            Click Me!
        </button>
    )
}

const ParentComponent = () => {
    const onClickHandler = () => {
        console.log('Hello From Parent Component!');
    }
    return (
        <ChildComponnet onClickFromParentComponnet={onClickHandler} />
    )
}
```


