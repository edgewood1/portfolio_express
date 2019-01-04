---
path: "/blog/rendering"
date: "2017-11-07"
title: "Rendering"
tags: ["React"]
published: true
---

**How do you render via React?**

Below are 11 ways.

But first, be sure to consider these three parts in the following files:

app.js (our component) will consist of 3 parts:

1. imports
2. element or component
3. export

index.js will handle rendering:

1. import element / component
2. render

__Method 1: JSX variables or “elements”__

Below, we do two things:

1. app.js defines a JSX element and exports it
2. index.js imports it and renders it to id="root" in our index.html file

What is the default?

app.js

```javascript
const element = <h1> Damn! </h1>;
export default element;
```

index.js

```javascript
import React from "react";
import ReactDOM from "react-dom";
import element from "./app";

ReactDOM.render(element, document.getElementById("root"));
```

**_From elements to Functions_**

_In the following examples, we will use exclude the imports and the file names._

**Method 2: Component Functions w/o Arguments**

There are two kinds of components:

1. “component” functions, or “stateless components” (see below)
2. class components

Below, we write a component function that returns a JSX element.
Then, we reference this component as the first argument in our render method: <Element />

_We represent components via a capital initial enclosed in a self-closing tag. This is called a **component tag** _

```javascript
function Element() {
  return <h1> Damn! </h1>;
}
export default Element;

ReactDOM.render(<Element />, document.getElementById("root"));
```

This is the same function in ES6:

```javascript
var Element = () => {
  return <h1> Damn! </h1>;
};

export default Element;
```

**Method 3: Component Functions w/ Argument**

This accomplishes the same as above, but adds an argument, called props.
Props is an object to be included in the return.

We represent this function as the component tag <Element />
Within this tag, we define the variable we will pass in.

This variable will be accessed as a property on the props object.

Finally, we include this variable in our render function.

```javascript
var Element = props => {
  return <h1> Damn! {props.name}</h1>;
};

const now = <Element name="Jack" />;

export default now;

ReactDOM.render(now, document.getElementById("root"));
```

**Method 4: Nested Component Functions w/ Argument**

Below we write two component functions:

- Welcome accepts a name property "Hello ... <name> "
- App returns Welcome four times, assigning a new name each time.

App contains Welcome, so:

- App is the parent component. It assigns value to the prop.
- Welcome is the child component. It renders the prop.

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

export default App;

ReactDOM.render(<App />, document.getElementById("root"));
```

**Method 5: Component Classes**

Below, the class “copies” the properties and methods of React.Component module
To this, it adds our JSX rendering.

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello </h1>;
  }
}

export default Welcome;

ReactDOM.render(<Welcome />, document.getElementById("root"));
```

**_From Functions to Classes_**

**Method 6: Using Property**

Property is an object used to pass data from a parent to a child.

Child - returns a property
Parent - returns Child along with an assigned property (example="foo").

All variable references, such as to state, occur in curly braces.

```javascript
class Parent extends React.Component {
  render() {
    return <Child example="foo" />;
  }
}

class Child extends React.Component {
  render() {
    return <h1>{this.props.example}</h1>;
  }
}

export default Parent;

ReactDOM.render(<Parent />, document.getElementById("root"));
```

_We could also include each class in its own file, exporting Child to Parent, and Parent to index.js, where it'd be rendered._

**Method 7: Using State**

Unlike component functions, component classes allows input of a private internal “state”.

State is an object, which can be changed, and when it does, the component auto re-renders to show the new state value.

Props cannot be changed

In the code below, parent renders its internal state while passing a property to be rendered in the Child.

```javascript
class Parent extends Component {
  state = {
    count: 0,
    saying: "Hi"
  };
  render() {
    return (
      <div>
        <h1>
          I said "{this.state.saying}" {this.state.count} times
        </h1>
        <Child example="foo" />;
      </div>
    );
  }
}

export default Parent;

ReactDOM.render(<Parent />, document.getElementById("root"));
```

**Method 8: Props and States in Child class**

State is internal to each component, so the Parent and Child will each maintain their own state.

In the Parent, we:
render an internal state while passing a property to be rendered in the Child.

In the Child, we:
render a unique internal state while rendering property sent by the Parent

```javascript
class Parent extends React.Component {
  state = {
    count: 0,
    saying: "Hi"
  };
  render() {
    return (
      <div>
        <h1>
          I said "{this.state.saying}" {this.state.count} times
        </h1>
        <Child example="foo" />
      </div>
    );
  }
}

class Child extends React.Component {
  state = {
    count: 3,
    saying: "Yo"
  };
  render() {
    return (
      <div>
        <h1>Inherited from Parent: {this.props.example}</h1>
        <h1>
          Defined by Child: I said "{this.state.saying}" {this.state.count}
          times
        </h1>
      </div>
    );
  }
}

ReactDOM.render(<Parent />, document.getElementById("root"));
```

Method 9: Changing the Child state via a button click

setState() – used to change the state
onClick – the eventListener:
Child does the following:

assigns the state (state.count = 0)
creates a new function (event handler) that adds 1 to state.count
returns a button, which, when clicked, will fire the now function and display the current count
The parent renders the Child, passing in the props.title value

```javascript
class Parent extends React.Component {
  render() {
    return <Child title="Welcome" />;
  }
}

class Child extends React.Component {
  state = {
    count: 0
  };

  now = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <div>
        <h1> Title: {this.props.title} </h1>
        <h1> Counter: {this.state.count} </h1>
      </div>
    );
  }
}

ReactDOM.render(<Parent />, document.getElementById("root"));
```

method 10: using constructor()

method 11: passing from Child to Parent (reverse of the norm) using a callback:

```javascript
class Parent extends React.Component {
  myCallback = dataFromChild => {
    console.log(dataFromChild);
  };

  render() {
    return <Child callbackFromParent={this.myCallback} />;
  }
}

class Child extends React.Component {
  state = {
    value: ""
  };

  someFn = e => {
    e.preventDefault();
    var newInput = e.target.value;
    this.setState({
      value: newInput
    });

    this.props.callbackFromParent(this.state.value);
  };

  some = e => {
    this.setState({ value: e.target.value });
  };

  render() {
    return (
      <form onSubmit={this.someFn}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.some} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}

ReactDOM.render(<Parent />, document.getElementById("root"));
```

Process
Parent calls child, passing a callback in callbackFromParent
Child renders form whose value is this.state.value or “” (and is thus controlled)
on Change, this.state.value becomes whatever is entered
on Submit, this.state.value is sent to callback via SomeFn(). This callback had been stored in this.props.callbackFromParent. It will console.log the submitted text.
