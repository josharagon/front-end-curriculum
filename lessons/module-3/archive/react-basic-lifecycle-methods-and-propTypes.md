---
title: "React: Basic Lifecycle Methods and propTypes"
module: 3
---

## Agenda

- Discuss component constructor and super
- Learn about common lifecycle methods
- Learn about props, and prop-types

## Learning Goals

- Be able to explain what the constructor does in a component
- Be able to explain what super is, and why we have to call it
- Be able to explain when componentDidMount is called, and what we might use it
  for
- Be able to explain how getDerivedStateFromProps is used, and when we might use
  it
- Be able to explain what PropTypes are, why they are important, and what
  problem they solve for us

## Vocab

- super
- lifecycle methods
- componentDidMount
- getDerivedStateFromProps
- PropTypes

## Basic React Lifecycle Methods

A React Component goes through 3 phases during its Lifecycle:

* Mounting (aka Birth)
* Updating (aka Growth)
* Unmounting (aka Death)

Let's focus on the Mounting phase right now.

## Mounting

### constructor() && super()

Let's talk about the first methods we see in a class-based React component.  

```js
constructor() {
  super();
  this.state = {
    name: ''
  };
}
```

Can you think of where you've seen these methods before, outside of React? Take a minute to write down everything you know about `constructor()` and `super()` methods in JavaScript classes (think back to the beginning of Mod 2!).

Per [the docs](https://facebook.github.io/react/docs/react-component.html#constructor), the `constructor()` method is called once, and before the component is mounted onto the DOM. It is the first and only function called automatically whenever a `class`-based component is created.  

Any class in JavaScript has this rule: Within the constructor, it's important to immediately call `super()` in cases where our class extends any other class that also has a defined constructor. In the case of React class-based components, what class are we extending?

(Hint: go take a look at your last React project and find a class-based component. What does code say when you are declaring your component?)

Invoking `super()` will run the constructor method of the parent class and allows it to initialize itself. This invocation allows `this` to have a defined value **within the constructor**. Constructors are great for setting up our component and initializing state. However, this is **NOT** a place you should consider making a network request.

This does not mean that every class NEEDS a constructor. The [default constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor#Default_constructors) is used if you aren't modifying it. There is even an [eslint rule](http://eslint.org/docs/rules/no-useless-constructor) for detecting the use of a default constructor. Basically, if you don't need to initialize state or bind any methods, you don't need to implement a constructor for your component! It will get called automatically behind the scenes. Magic!

Rule of thumb, though, is typically: if you have a constructor method in your code, you must invoke the super method. In fact, browsers today will throw an error if you are using ES6 syntax and try to call a constructor method without `super()`! Thanks for looking out like a pal, browsers!

Let's take a minute to fire up a react component and watch the errors trigger as we build out a class based component. Remember - errors are trying their best to be helpful. They're like friendly goblins - kinda alarming to look at at first, but then you start to see how helpful they are.

Start a blank react project - feel free to use `create-react-app` for this lesson.  

You'll notice that React has some incredibly verbose and helpful goblins - I mean, error messages. Make sure you take time to read these as you build out projects this mod, they will almost always point you in the direction of happiness.  

### What is this `super()` business?  

Try this in your component and check out the error message:

```js
// Card.js

constructor(){
  console.log(this);
}
```

You should see something like this:  

```js
Failed to compile.

Error in ./src/Card.js
Syntax error: 'this' is not allowed before super()

`
  3 | export default class Card extends Component {
  4 |   constructor(){
> 5 |     console.log(this);
    |                 ^
  6 |   }
  7 |
  8 |   render () {
`
```

If, in your constructor, you need access to that component's `props`, you _must_ pass these as an argument down through `super()`.

```js
constructor(props) {
  super(props);
  this.state = {
    name: props.initialName
  };
}
```

Without passing the props down into this method, `this.props` will return as `undefined` **within the `constructor` method**.  
This is bad because the constructor method is the first function called when your component is instantiated as a class - knowing the context of `this` from the get go can be a big deal.   

Note that whether or not you have a constructor method has no effect on `this` or `this.props` within the `render()` method - the `render()` method defines its own context.  

Per [the docs](https://reactjs.org/docs/state-and-lifecycle.html#adding-local-state-to-a-class), class components should always pass props to the base constructor (this is why we pass them into the `super()` method). However there is some debate as to why this is suggested, since React will automatically set the props for you once the constructor has fired.

### static getDerivedStateFromProps() - ***NEW***

This is a new lifecycle method that was added with React 16.3. Check out what the [docs](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops) have to say about it. Because this is a static method, we don't have access to `this` inside the method. Per the docs, this method gets invoked under 2 circumstances:
  * after a component is instantiated
  * when the component receives new props

It takes in `nextProps` and `prevState` and returns an object to update state, or null if the new props do not require any state updates.

### componentWillMount() - ***deprecated***

There is not a huge difference between `constructor()` and `componentWillMount()`. Like all other lifecycle methods withing the Birth/Mounting phase, `componentWillMount()` is only called once. Historically, there were some reasons to use `componentWillMount()` over `constructor()`, but that practice has since been deprecated. You should just know that it exitsts, but **DON'T USE IT**

### render()

Now that our component has been initialized and configured, we can begin rendering some content/elements on the page. This is the lifecycle method that React developers are most familiar with. It is the one lifecycle method that exists across all phases of a React component. It's important to remember to always keep `render()` a **pure method**. This means we should never call `this.setState()` within `render()`. If we take a minute to think about this, it makes total sense... it would put us in an infinite loop of re-rendering!  

#### Managing Children Components and Mounting

The React Element that has been rendered by the initial `render()` may possibly have a number of children that need to be rendered as well. This is where each of those children kickoff their own lifecycle methods... `constructor()`, `render()`, and `componentDidMount()`. Once all the children have been successfully created, initialized and mounted, the parent's `componentDidMount()` method will finally be called.

### componentDidMount()

Per [the docs](https://reactjs.org/docs/react-component.html#componentdidmount), `componentDidMount()` is invoked "immediately
after a component is mounted." When `componentDidMount()` is called, this signalizes that the component - and all its sub-components - have rendered properly. Any functionality that is dependent on existing DOM nodes should live here. For example, let's say you want to set state which affects a `<p>` tag, you need to wait until that `<p>` tag exists before you can throw any additional information into it.

This is also the go-to location to fire off an API call or network request (**BEST PRACTICE**). This is a great [blog post](https://www.robinwieruch.de/react-fetching-data/) that discusses how and where to fetch data in React.

**NOTE:** Setting state in this method **WILL** trigger a re-render - but will not cause an infinite re-rendering loop, because `componentDidMount` only fires after the initial mount of the component.

## More About Props

### PropTypes

PropTypes allow you to specify what type of props you are expecting in a certain component. This is also known as "typechecking".

Many people have moved to implementing languages like [TypeScript](https://www.typescriptlang.org/) or [Flow](https://flowtype.org/) for this exact purpose, but even without any additional technologies, the `prop-type` module lets you establish a safety net with very little effort.

The overall benefit of using PropTypes is that it gives your error-message goblins more tools to provide you with helpful, specific errors to better debug your code.

Let's say you declare a component `<Main title={this.state.title}/>`. Here, your component is expecting a prop called `title` and you (probably) expect it to be a string. If you define that value within your `propTypes` object and it comes in as something else - say for example the API you are consuming has changed and now you have an array of strings - you will get a helpful warning message in your console.  

Previously, PropTypes was built into the React library itself, however it has since been extracted into its own module. You can install it like any other module.

```sh
$ npm install prop-types -S
```

_hint: the `-S` flag is the same as the `--save` flag, and as of npm version 5, all dependencies installed without a flag default to being saved as regular dependencies._

In React, `PropTypes` are declared like this:

```js
// Main.js

import PropTypes from 'prop-types'

class Main extends Component  {
  render() {
    return(
      <p>Welcome, {this.props.name}</p>
    )
  }
}

Main.propTypes = {
  name: PropTypes.string
}
```

The error you will see if the component gets something besides a string would look something like this:  

```
Warning: Failed prop type: Invalid prop `name` of type `array` supplied to `Main`, expected `string`.
    in Main (created by App)
    in App
```

Without using PropTypes, you would have seen a vague error when the Component failed to render. With PropTypes, we can see that the error was that we were receiving an array when we expected a string.

### Class propTypes

Check out a complete list of `propTypes` and examples of usage [here](https://facebook.github.io/react/docs/typechecking-with-proptypes.html#react.proptypes).  

By default, all props specified within the `Class.propTypes` object will be considered optional. There are many instances where your component will not function correctly without that particular prop. To add a validation that will fire an error message if a prop does not show up at all, simply add `.isRequired` to the end of the propType declaration.  

```js
Main.propTypes = {
  name: PropTypes.string.isRequired
}
```

You can also be more generic - let's say you need a prop to come in but it doesn't matter what type it is as long as it's there. Instead of specifying a particular JS primitive you can use `.any`.

```js
Main.propTypes = {
  name: PropTypes.any.isRequired
}
```

### Your Turn

Take the next 5 minutes to look up the following prop types and understand what they do. We will circle back to talk about
these particular methods when you are done.  

- `PropTypes.oneOf()`  
- `PropTypes.arrayOf()`  
- `PropTypes.objectOf()`  
- `PropTypes.shape()`


## DefaultProps

Just like when writing functions, React also allows us to provide a default value for props. [defaultProps](https://facebook.github.io/react/docs/typechecking-with-proptypes.html#default-prop-values) let you ensure that a value will be passed through. This helps eliminate some of the incessant ternaries that either render the prop or an empty string, for instance.  

```js
class Main extends Component  {
  render() {
    return(
      <p>Welcome, {this.props.name}</p>
    )
  }
}

Main.defaultProps = {
  name: 'Batman'
}
```  

**Note:** The `propTypes` typechecking validations fire AFTER `defaultProps` have been resolved. This allows for the default props to fill themselves in before any warning messages are thrown.  

### Your Turn  

Now that we've talked about the most obvious use cases of propTypes to preemptively debug your code, read the following two articles - you are highly encouraged to take notes:  
- [Better Prop Validation](https://medium.com/@MoeSattler/better-prop-validation-in-react-cc83590d311f#.8z6wszfzn).  
- [Writing A Good React Component](https://thoughts.travelperk.com/writing-a-good-react-component-59624ed40b8e#.64wzjk4qc)  

We will circle back to review the main points of these articles together.  

When you are done, get with your project partner (if you have one) and implement propTypes and defaultProps into your current project.  
