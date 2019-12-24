## Introduction to React library (outdated)

> note from last year

[[dist](dist/)]

### Simple things

**Creating a first simple component in React**

```javascript
export default function SimpleComponent(props) {
    const hello = 'Hello, World!';

    return (
        return (<div>{hello}</div>);
    )
}
```

**Creating a standard component (based on ES6's Class)**

```javascript
import React, { Component } from "react";

export default class SomeComponent extends Component {
  constructor(props) {
    super(props);
  }

  render() {
    return <div>{this.props.hello}</div>;
  }
}
```

**Component state management**

```javascript
...
constructor() {

    // set initial state of any data
    this.state = {
        someData: false
    };

}

someMethod() {

    // changing state
    this.setState({
        someData: true
    });

}
```

### Prop-Types

**Set types for incoming component's props**

Prop-Types is another module. So, we need install this:

```
yarn add prop-types -S
```

Adding to component:

```javascript
import PropTypes from 'prop-types';
...

// specify what props.article should be object type
// isRequired - is required prop for this component
static propTypes = {
    article: PropTypes.object.isRequired
};
```

[[Typechecking With PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)]

### Decorators

> Decorator is a function which takes in parameter some class and returns this modified class (actually returns class-wrap over this class).

```javascript
import React from "react";

export default Component =>
  class WrappedComponent extends React.Component {
    state = {
      isOpen: false
    };

    render() {
      return (
        <Component
          {...this.props}
          isOpen={this.state.isOpen}
          toggleOpen={this.toggleOpen}
        />
      );
    }

    toggleOpen = e => {
      // e - is not a simple object Event, is wrap Proxy over Event
      // e.nativeEvent it's a simple Event object
      // async operation
      this.setState({
        isOpen: !this.state.isOpen
      });
    };
  };

/* it's same but not arrow function
export default function (Component) {
    return class WrappedComponent extends React.Component {
        render() {
            return <Component {...this.props} />
        }
    }
}
*/
```

**Using in component Article**

```javascript
import toggleOpen from '../decorators/toggleOpen';

class Article extends React.Component {
...


export default toggleOpen(Article);
```

### React lifecycle

- constructor(props)
- componentWillMount (deprecated)
- render
- componentDidMount
- componentWillReceiveProps (deprecated)

[[State and Lifecycle](https://reactjs.org/docs/state-and-lifecycle.html)]
