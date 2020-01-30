# React Notes 

## What is React? 
- an open source view library created by Facebook
- used to render the UI of modern web applicatiosn 
- uses exntension of JS called JSX which allows HTML to be written directly into the JS
  - allows to fully use JS within HTML and keeps it readable 
  - can write JS within JSX
  - `ReactDOM.render(JSX, document.getElementByID('root'));`
- note that nested JSX must only return one single element (wrap nested things all up in one div or something)
- note: all HTML attributes and event references become camelCase in JSX

## Components 
- component composition is one of react's powerful features
- good to modularize code and think of the UI as a bunch of components (the basic building blocks) 
  - helps to separate the UI code from the application logic code 
- **functional stateless component vs pure react components**
  - stateless component will re-render every time the state of the parent component changes 
  - pure component will only re-render every time a prop on which it depends on changes (considerd more stable?)
    - automatically implements "shouldComponentUpdate" for you 
- note: extending component vs using const (if need lifecycle events, extend the class)
- stateless functional component:
  - any fnction you write which accepts props and returns JSX
- stateless component: 
  - class that extends `React.Component`, but does not use internal state 
- stateful component:
  - class component maintains its own intenral state 
- NOTE: in general, we try to minimize statefulness so that state management only applies to a specific area of the application 

### Props 
- props/properties can be passed to child components 
- can create custom HTML attributes 
- arrays can also be passed as props
  - note that to pass an array to a JSX element, it must be treated as JS and so wrapped in curly braces
- **proptypes**: useful to ensure that components receive props of the correct type otherwise will throw an warning 
  - NOTE: PropTypes is now imported independantly from React ```JSX import PropTypes from 'prop-types';```

### State
- state is the data your application needs to know about, that can change over time 
- you have access to the state throughout the life of your component 
```JSX
class StatefulComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      // note: must create a class component by extending React.Component in order to create state like this 
      name: "Jenny"
    }
  }
  render() {
    return (
      <div>
        <h1>{this.state.name}</h1>
      </div>
    );
  }
}
```
- only the component itself is aware of its own state
  - the state is completely encapsulated/local
- another way to access state:
  - in the render() method, before the return statement, you can write JS directly 
    - so you can use values from props and states and everything and then assign them to variables which can then in turn be used in the return statement 
- state can be updated using `this.setState({name: "new string"});`
  - React expects you to never modify state directly but always through this method 
- NOTE: state updates may be asynchronous (multiple `setState()` calls may be batched together, so you can't rely on the previous value of `this.state` or `this.props` when calculating the next value)
ex: 
```JSX
// don't do this 
this.setState({
  counter: this.state.counter + this.props.increment  
});

// do this instead
this.set((state, props) => ({
 counter: state.counter + props.increment 
}));

// or if you only need state
this.set(state => ({
  counter: state.counter + 1
}));
```

EXAMPLE
```JSX
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      visibility: false
    };
    this.toggleVisibility = this.toggleVisibility.bind(this);
  }
  toggleVisibility() {
    this.setState(state => ({
      visibility: !state.visbility
    }));
  }
  render() {
    if (this.state.visbility) {
      return (
        <div>
          <button onClick={this.toggleVisibility}>Click Me</button>
          <h1>Now you see me!</h1>
        </div>
      );
    } else {
      return (
        <div>
          <button onClick={this.toggleVisibility}>Click Me</button>
        </div>
      );
    }
  }
};
```

#### Forms
- forms hold their own state in the DOM, changes can be automatically accessed 
- React can contorl the internal state for forms &rarr; so they are controleld components

### Passing State as Props to Child Components 
- **unidirectional data flow**:
  - state flows in one direction down the tree of your application's components (from stateful parent component to child components)
  - the child components only receive the state data they need 
- complex stateful apps can be broken down into just a few/a single stateful component
  - the rest of your components simply receive state from the parent as props and render a UI from that state 
  - separation: state management is handled in one part of code and UI rendering in another -> an important idea (splitting state logic from UI logic) 

- handler functions and methods can also be passed to child components 
  - allow the child to interact with their parent components 

## Lifecycle Methods
- lifecycle methods are methods that provide opportunities to perform actions at specific points in the lifecycle of a component
- examples:
  - `componentWillMount()`: called before the `render()` method when a component is being mounted to the DOM 
  - 'componentDidMount()`: called right after the component is mounted to the DOM 
    - any `setState()` calls here will cause a rerendering 
    - best place to attach any event listeners required for specific functionality 
      - NOTE: react wraps the native event system with a synthetic one, so events all behave the same across browsers 
**EXAMPLE **
```JSX
class MyComponent extends React.Component {
  constructor (props) {
    super(props);
    this.state = {
      message: ''
    };
    this.handleEnter = this.handleEnter.bind(this);
    this.handleKeyPress = this.handleKeyPress.bind(this);
  }
  componentDidMount() {
    document.addEventListener("keydown", this.handleKeyPress)
  }
  componentWillUnmount() {
    document.removeEventListener("keydown", this.handleKeyPress)
  }
  handleEnter() {
    this.setState({
      message: this.state.message + "You pressed the enter key!"
    });
  }
  handleKeyPress(event) {
    if (event.keyCode === 13) {
      this.handleEnter();
    }
  }
  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
      </div>
      );
  }
};
```

  - `shouldComponentUpdate(nextProps, nextState)` [returns boolean]: called when child components receive new state or props and declare whether the components should update or not 
    - good for optimizing performance; default behaviour is to re-render every time it receives new props

## Inline Styles 
- instead of styling with a stylesheet, inline styles are common 
ex. 
```JSX 
<div style={{color: "yellow", fontSize: 16}}>Mellow Yellow</div>
// can use fontSize: "16px" too or "2em", but if written as a number, px is assumed 

// using a "styles" object
const styles = {
  color: "purple", 
  fontSize: 40,
  border: "2px solid purple"
};
<div style={styles}>Purple with border</div>
```

- NOTE: JS can be used in the `render()` method before the `return()` method to either control what is returned or to do logic and stuff; the return statement can access the variables/constants through {}

## Conditional Rendering 

ex. 
```JSX
{this.state.boolean && <h1>the boolean is true!</h1>}
// can put this in the return statement and if the boolean is true, then the <h1> tag will be returned 
```

### ternary operators 
```JSX
// basic syntax 
condition ? expressionIfTrue : expressionIfFalse

// multiple expressions
condition1 ? if1True : condition2 ? if2true : if2false
```

### Render Condtionally From Props 
- combine logic from above to conditionally render elements depending on props 
- or conditionally change inline CSS based on component state 
  - paradign is a dramatic shift from the traditional approach of applying styles by modifying DOM elements directly (you'd have to keep track of when they change and handle direct manipulation too)
  - in react, it is preferred to have a clear flow of information in only one direction 

### Array.map()
```JSX
this.state.myArray.map(i => <li>{i}</li>)
```

### Array.filter()
- filters an array based on a condition and returns a new array with the passing elements 

## Render React on the Server
- reasons for rendering on server:
  - without doing this, React apps would consist of relatively empty HTML file and a large bundle of JS -> not ideal for search engines indexing the site 
  - faster initial page load experience since rendered HTML is smaller than the JS code of the entire app 
    - react will manage it after the initial load 
```JSX
ReactDOMServer.renderToString(<App/>)
```
