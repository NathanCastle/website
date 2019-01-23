# Daily Journal

The journey to update my tech skills is off to a slow start. I'm a bit behind on hours at work, so I've been working into the evenings every day. If I get home within an hour of my bed time, I don't start anything. 

Now its the weekend and I have some free time this morning.

## Continuing Journey

Its been a bit since I did the tutorial, so I'm starting by reviewing what I did and adding comments.

Now that I've refreshed my memory from the tutorial, I'm moving on to the [Hello World](https://reactjs.org/docs/hello-world.html) guide.

### Notes

* Component names must start with capital letters - DOM tags start with lowercase
* Components must not change their own props - hard rule
   * Components are allowed to update their own local state
* Components have lifecycle methods - `componentDidMount` and `ComponentWillUnmount`
    * Useful for cleaning up events, etc. when a component is being destroyed
* When `setState` is used to update state, React detects that and knows to update the UI
* React can mess with order of `setState` calls; if previous state is required, use an arrow function to set state
```jsx
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```
* `this` isn't bound by default in callbacks/event handlers. The class constructor should bind them explicitly: `this.handleClick = this.handleClick.bind(this);`
* Controlled components - you can manually handle changes in state on form elements to make sure state stays consistent with the react state.
    * Set the value using React, then use an event handler to call `setState` when user inputs; this will ensure React is in control
    * See https://reactjs.org/docs/forms.html
* Lifting state up to common components can mean more boilerplate than traditional two-way binding, but makes it easier to find bugs (according to the authors)
* Instead of using inheritance, use composition
    * E.g. specialized components just render the common components with specific properties set