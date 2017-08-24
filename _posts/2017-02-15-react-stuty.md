---
layout: post
title:  react学习
categories: 前端开发
tag: react
---

* content
{:toc}

## 遇到的问题
- npm install -g create-react-app 遇到的问题：Npm Please try using this command again as root/administrator
    - 解决方案：```sudo chown -R $USER /usr/local```

- 

## 学习过程
- 学习Create React App－－https://github.com/facebookincubator/create-react-app#getting-started

## 知识点
- a modern build pipeline typically consisits of: a package manager, such as yarn or npm, to manager third-party packages;a bundler, sunch as webpack or browserify, to bundle code into small packages to optimize load time; a compiler such as babel. 
- building blocks of React apps: elements and components
- null--->non-value; undefined--->uninitialized value **[js内容]**
- component
	- one write style

	```
function test(props) {
return <p>hello,{props.name}</p>
}
	```

	- second write style(ES6)


	```
class test extends React.Component {
	render(){
return <p>hello, {this.props.name}</p>
	}
}
	```

- components defined as classes have some additional features --> local state is a feature only available to classes.
- All React components must act like pure functions with respect to their props--pure functions means that they do not attempt to change their inputs, and always return the same result for the same inputs.
- State is similar to props, but it is private and fully controlled by the component.---that means you should always use class rather than function because only class can use state

### **Converting a function to a class**

1\Create an ES6 class with the same name that extends React.Component.

2\Add a single empty method to it called render().

3\Move the body of the function into the render() method.

4\Replace props with this.props in the render() body.

5\Delete the remaining empty function declaration.


```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

### lifecycle control

	- like the code, ReactDOM.reader first run, then, ->the Clock constructor->the Clock render function to draw the dom->after that, componentDidMount set a timer to call tick every minite, if Clock component is removed from DOM, componentWillUnmout called.

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

- this.props and this.state may be updated asynchronously, so you cannot rely on their values to calcute the next state

```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

``to fix this``  use a second form of setState() that accepts a function rather than an object

```
// Correct--ARROW FUNCTION
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```
**OR**

```
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### handling events: dom vs. react

- lowercase vs. camelCase

- pass function as a string vs. as a event handler

```
<button onclick="activateLasers()">
  Activate Lasers
</button>
VS.
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

- return false to prevent default behavior vs.  call preventDefault explicitly

```
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>

vs.

function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}

```

-  in JavaScript, true && expression always evaluates to expression, and false && expression always evaluates to false. **[JS内容]**
- don't recommend using indexes for keys if the items can reorder, as that whould be slow
- A good rule of thumb is that elements inside the map() call need keys.