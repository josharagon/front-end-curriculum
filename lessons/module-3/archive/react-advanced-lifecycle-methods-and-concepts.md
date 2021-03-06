---
title: React Additional Lifecycle Methods & Concepts
module: 3
tags: React
---

## Agenda

- Break into groups, learn about individual lifecycle methods
- Teach each lifecycle method to the larger group
- Learn about `componentDidCatch`, and how to use it

## Learning goals

- Be able to identify when each lifecycle method is called
- Be able to give an example of how you might use each lifecycle method

## Vocab

- getDerivedStateFromProps
- shouldComponentUpdate
- componentWillUpdate
- getSnapshotBeforeUpdate
- componentDidUpdate
- componentWillUnmount
- componentDidCatch

## LifeCycle Methods: An Interactive Adventure

The React team is working on **async rendering**. Per the [docs](https://reactjs.org/blog/2017/09/26/react-v16.0.html#new-core-architecture), this is "a strategy for cooperatively scheduling rendering work by periodically yielding execution to the browser. The upshot is that, with async rendering, apps are more responsive because React avoids blocking the main thread." This is going to play a big part in the future of React. In anticipation of this release, React has begun making some changes that will help prepare us for async rendering. Among those changes are getting rid of some lifecycle methods that are now considered UNSAFE to use and adding a few new lifecycle methods.

We've already talked about the Mounting (aka Birth) Phase that a class based React component goes through when it is created. Now, we are going to spend a little time learning about the other two phases of a component's lifecycle. You're going to split up into groups and each group will be given a lifecycle method to research and learn about. You will then teach the rest of the class about your assigned lifecycle method. There are a number of resources at the bottom of this lesson that you can use, but by no means are you limited to just the ones here.

## Phase 1: Mounting (see [previous lesson](react-basic-lifecycle-methods-and-propTypes.html))

### constructor()

You've seen this before... this is where we call the inherited constructor (`super`), and set our initial state.

### componentWillMount() - *will be deprecated with React 17*

This legacy method is now **unsafe** to use and should be replaced with `componentDidMount()`.

### static getDerivedStateFrom Props - *NEW with React 16.3.0*

See the explaination in growth/updating.

### render()

Yup, render is a lifecycle method. You should be returning JSX from it. Also, render isn't the place for any complex logic. Keep it as dumb as possible.

### componentDidMount()

`componentDidMount` is just called one time, in this birth/mounting process. This is important to remember when you're making API calls. If you need to make an API call *after* the component has already called `componentDidMount`, you'll need to do it somewhere else.

## Phase 2: Updating (Growth)

### componentWillReceiveProps() - *will be deprecated with React 17*

This legacy method is now **unsafe** to use, and should be replaced with `static getDerivedStateFromProps()`.

### static getDerivedStateFromProps - *NEW with React 16.3.0*

This method exists for only one purpose... it enables a component to update its internal state as the result of changes in props. Since this method is static, you can't refernece `this` in it. Whatever object is returned from this method is what the state of the component will be set to. This new method will be called on the initial mounting of the component, as well as when it's re-rendered.

### shouldComponentUpdate()

`shouldComponentUpdate` returns true or false, and is an opportunity to optimize your application. If `shouldComponentUpdate` returns false, the  rest of the lifecycle methods will not fire (i.e. render). Think about why you might want this behavior.

### componentWillUpdate() - *will be deprecated with React 17*

This legacy method is now **unsafe** and shouldn't be used going forward.

### getSnapshotBeforeUpdate - *NEW with React 16.3.0*

This method is called right before any DOM manipulations are made. If you need to do any calculations that you'd like to use in componentDidUpdate(), you can return those in this method, and they will get passed along as a the third argument to componentDidUpdate()

### render()

The same render as in Birth/Mounting phase

### componentDidUpdate()

Per the docs: "Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to the previous props (e.g. a network request may not be necessary if the props have not changed)."

## Phase 3: Unmounting (Death)

### componentWillUnmount()

This is invoked just once, before the component is unmounted and destroyed. It's a good place to invalidate any open network requests, kill any timers, and do any other necessary cleanup tasks.

## Error Handling

### componentDidCatch() - *just know that this exists*

`componentDidCatch` allows you to set up error boundaries in your application, and display fallback UI rather than just letting the error crash into the console. Check the references below for more information.

## References

- [React Lifecycle Docs](https://reactjs.org/docs/react-component.html)
- [The React Life Cycle](https://developmentarc.gitbooks.io/react-indepth/content/life_cycle/introduction.html)
- [Understanding Component Lifecycle](https://medium.com/@baphemot/understanding-reactjs-component-life-cycle-823a640b3e8d)
- [Lifecycle Simulators](https://reactarmory.com/guides/lifecycle-simulators)
- [Dan Abramov's Twitter chart/convo on modern React lifecycle methods](https://twitter.com/dan_abramov/status/981712092611989509)
- [Error handling in React 16](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)
- [Update on Async Rendering](https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html)
- [React v16.3.0: New Lifecycles](https://reactjs.org/blog/2018/03/29/react-v-16-3.html)
