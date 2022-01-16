# React.js cheatsheet

## Components
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
class Hello extends React.Component {
  render () {
    return <div className='message-box'>
      Hello {this.props.name}
    </div>
  }
}
const el = document.body
ReactDOM.render(<Hello name='John' />, el)
```

## Import multiple exports
```jsx
import React, {Component} from 'react'
import ReactDOM from 'react-dom'
class Hello extends Component {
  ...
}
```

## Properties
- Use this.props to access properties passed to the component.
```jsx
<Video fullscreen={true} autoplay={false} />
render () {
  this.props.fullscreen
  const { fullscreen, autoplay } = this.props
  ···
}
```

## States
- Use states (this.state) to manage dynamic data.
```jsx
constructor(props) {
  super(props)
  this.state = { username: undefined }
}
this.setState({ username: 'rstacruz' })
render () {
  this.state.username
  const { username } = this.state
  ···
}
```

## Children
- Children are passed as the children property.
```jsx
<AlertBox>
  <h1>You have pending notifications</h1>
</AlertBox>
 
class AlertBox extends Component {
  render () {
    return <div className='alert-box'>
      {this.props.children}
    </div>
  }
}
```

## Nesting
- Nest components to separate concerns.
```jsx
class Info extends Component {
  render () {
    const { avatar, username } = this.props

    return <div>
      <UserAvatar src={avatar} />
      <UserProfile username={username} />
    </div>
  }
}

import React, {
  Component,
  Fragment
} from 'react'

class Info extends Component {
  render () {
    const { avatar, username } = this.props

    return (
      <Fragment>
        <UserAvatar src={avatar} />
        <UserProfile username={username} />
      </Fragment>
    )
  }
}
```

# Defaults

## Setting default props
```jsx
Hello.defaultProps = {
  color: 'blue'
}
```

## Setting default state
```jsx
class Hello extends Component {
  constructor (props) {
    super(props)
    this.state = { visible: true }
  }
}
```

# Other components

## Functional components
- Functional components have no state. Also, their props are passed as the first parameter to a function.
```jsx
function MyComponent ({ name }) {
  return <div className='message-box'>
    Hello {name}
  </div>
}
```

## Pure components
- Performance-optimized version of React.Component. Doesn’t rerender if props/state hasn’t changed.
```jsx
import React, {PureComponent} from 'react'

class MessageBox extends PureComponent {
  ···
}
```

## Component API
- These methods and properties are available for Component instances.
```jsx
this.forceUpdate()
this.setState({ ... })
this.setState(state => { ... })
this.state
this.props
```

# Lifecycle

## Mounting
- Set initial the state on constructor(). Add DOM event handlers, timers (etc) on componentDidMount(), then remove them on componentWillUnmount().
```jsx
constructor (props)	//Before rendering
componentWillMount()	//Don’t use this
render()	//Render
componentDidMount()	//After rendering (DOM available)
componentWillUnmount()	//Before DOM removal
componentDidCatch()	//Catch errors (16+)
```

## Updating
- Called when parents change properties and .setState(). These are not called for initial renders.
```jsx
componentDidUpdate (prevProps, prevState, snapshot)	//Use setState() here, but remember to compare props
shouldComponentUpdate (newProps, newState)	//Skips render() if returns false
render()	//Render
componentDidUpdate (prevProps, prevState)	//Operate on the DOM here
```

# Hooks (New)

## State Hook
- Hooks are a new addition in React 16.8.
```jsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

## Declaring multiple state variables
```jsx
function ExampleWithManyStates() {
  // Declare multiple state variables!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

## Effect hook
- If you’re familiar with React class lifecycle methods, 
- you can think of useEffect Hook as componentDidMount, componentDidUpdate, and componentWillUnmount combined.
- By default, React runs the effects after every render — including the first render.
```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

## Building your own hooks
```jsx
import React, { useState, useEffect } from 'react';

function useFriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  }, [props.friend.id]);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

```jsx
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

## Hooks API Reference
- basic hooks
```jsx
useState(initialState)	 
useEffect(() => { … })	 
useContext(MyContext)	//value returned from React.createContext
```
- additional hooks
```jsx
useReducer(reducer, initialArg, init)	 
useCallback(() => { … })	 
useMemo(() => { … })	 
useRef(initialValue)	 
useImperativeHandle(ref, () => { … })	 
useLayoutEffect	//identical to useEffect, but it fires synchronously after all DOM mutations
useDebugValue(value)	//display a label for custom hooks in React DevTools
```

# DOM nodes
## References
- Allows access to DOM nodes.
```jsx
class MyComponent extends Component {
  render () {
    return <div>
      <input ref={el => this.input = el} />
    </div>
  }

  componentDidMount () {
    this.input.focus()
  }
}
```

## DOM Events
- Pass functions to attributes like onChange.
```jsx
class MyComponent extends Component {
  render () {
    <input type="text"
        value={this.state.value}
        onChange={event => this.onChange(event)} />
  }

  onChange (event) {
    this.setState({ value: event.target.value })
  }
}
```

# Other features
## Transferring props
- Propagates src="..." down to the sub-component.
```jsx
<VideoPlayer src="video.mp4" />

class VideoPlayer extends Component {
  render () {
    return <VideoEmbed {...this.props} />
  }
}
```
## Top-level API
- There are more, but these are most common.
```jsx
React.createClass({ ... })
React.isValidElement(c)
ReactDOM.render(<Component />, domnode, [callback])
ReactDOM.unmountComponentAtNode(domnode)
ReactDOMServer.renderToString(<Component />)
ReactDOMServer.renderToStaticMarkup(<Component />)
```

# JSX patterns
## Style shorthand
```jsx
const style = { height: 10 }
return <div style={style}></div>
return <div style={{ margin: 0, padding: 0 }}></div>
```
## Inner HTML
```jsx
function markdownify() { return "<p>...</p>"; }
<div dangerouslySetInnerHTML={{__html: markdownify()}} />
```
## Conditionals
```jsx
<Fragment>
  {showMyComponent
    ? <MyComponent />
    : <OtherComponent />}
</Fragment>
```
## Lists
- Always supply a key property.
```jsx
class TodoList extends Component {
  render () {
    const { items } = this.props

    return <ul>
      {items.map(item =>
        <TodoItem item={item} key={item.key} />)}
    </ul>
  }
}
```
## Short-circuit evaluation
```jsx
<Fragment>
  {showPopup && <Popup />}
  ...
</Fragment>
```

# New features

## Returning multiple elements
- You can return multiple elements as arrays or fragments.
```jsx
render () {
  // Don't forget the keys!
  return [
    <li key="A">First item</li>,
    <li key="B">Second item</li>
  ]
}
```
```jsx
render () {
  // Fragments don't require keys!
  return (
    <Fragment>
      <li>First item</li>
      <li>Second item</li>
    </Fragment>
  )
}
```
## Returning strings
```jsx
render() {
  return 'Look ma, no spans!';
}
```
## Portals
- This renders this.props.children into any location in the DOM.
```jsx
render () {
  return React.createPortal(
    this.props.children,
    document.getElementById('menu')
  )
}
```
## Errors
- Catch errors via componentDidCatch. (React 16+)
```jsx
class MyComponent extends Component {
  ···
  componentDidCatch (error, info) {
    this.setState({ error })
  }
}
```
## Hydration
- Use ReactDOM.hydrate instead of using ReactDOM.render if you’re rendering over the output of ReactDOMServer.
```jsx
const el = document.getElementById('app')
ReactDOM.hydrate(<App />, el)
```

# Property validation
## PropTypes
```jsx
import PropTypes from 'prop-types'
```

```
any	//Anything
```
- Basic
```
string	 
number	 
func	//Function
bool	//True or false
```
- Enum
```
oneOf(any)	//Enum types
oneOfType(type array)	//Union
```
- Array
```
array	 
arrayOf(…)
```	 
- Object
```
object	 
objectOf(…)	//Object with values of a certain type
instanceOf(…)	//Instance of a class
shape(…)
```	 
- Elements
```
element	//React element
node	//DOM node
```
- Required
```
(···).isRequired	Required
```

## Basic types
```jsx
MyComponent.propTypes = {
  email:      PropTypes.string,
  seats:      PropTypes.number,
  callback:   PropTypes.func,
  isClosed:   PropTypes.bool,
  any:        PropTypes.any
}
```

## Enumerables (oneOf)
```jsx
MyCo.propTypes = {
  direction: PropTypes.oneOf([
    'left', 'right'
  ])
}
```

## Custom validation
```jsx
MyCo.propTypes = {
  customProp: (props, key, componentName) => {
    if (!/matchme/.test(props[key])) {
      return new Error('Validation failed!')
    }
  }
}
```

## Required types
```jsx
MyCo.propTypes = {
  name:  PropTypes.string.isRequired
}
```

## Elements
```jsx
MyCo.propTypes = {
  // React element
  element: PropTypes.element,

  // num, string, element, or an array of those
  node: PropTypes.node
}
```

## Arrays and objects
- Use .array[Of], .object[Of], .instanceOf, .shape.
```jsx
MyCo.propTypes = {
  list: PropTypes.array,
  ages: PropTypes.arrayOf(PropTypes.number),
  user: PropTypes.object,
  user: PropTypes.objectOf(PropTypes.number),
  message: PropTypes.instanceOf(Message)
}
MyCo.propTypes = {
  user: PropTypes.shape({
    name: PropTypes.string,
    age:  PropTypes.number
  })
}
```