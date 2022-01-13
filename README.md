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