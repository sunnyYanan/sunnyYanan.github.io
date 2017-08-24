---
layout: post
title:  react官网学习教程翻译
categories: 前端开发
tag: react
---

* content
{:toc}

[官网地址](https://facebook.github.io/react/docs/lists-and-keys.html)

# 快速开始

## 列表和关键字－Lists and Keys

&emsp;首先，让我们回顾下在javascript中你是如何转换列表的。
&emsp;给出下面的代码，我们使用map()方法将一个数组numbers的值变为其值的2倍。我们将这个转换结果赋值给变量doubled，并控制台输出它。

```
const numbers=[1,2,3,4,5];
const doubled = numbers.map((number)=>number*2);
console.log(doubled);
```

&emsp;这段代码将会将[2,4,6,8,10]输出到控制台。

&emsp;在react中，将数组转换成列表是基本一样的。

### 渲染多个组件－Rendering Multiple Components

&emsp;你可以创建元素的集合，并使用大括号{}将他们引入jsx中。

&emsp;下面代码中，我们使用map()方法循环numbers数组。对每个元素，返回一个<li>元素。最终，将元素的结果数组赋值给listitems：

```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);
```

&emsp;我们将整个listItems数组包进一个<ul>元素中，将其渲染到dom中：

```
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

&emsp;这个代码显示1到5的数字列表。

#### 基础列表组件－Basic List Component

&emsp;通常，你会在组件中渲染列表。

&emsp;我们可以将之前的例子变成一个组件，这个组件接收一个数组numbers，输出一个无序的元素列表。

```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

&emsp;当你运行这段代码，你会收到一个warning，告诉你列表元素应该有一个关键字。关键字“key”是当你创建元素列表时需要包含的一个特殊的字符串属性。我们在下一节讨论为什么它是重要的。

&emsp;让我们给numbers.map()中的列表元素赋一个关键字，来解决这个字符串丢失问题。

```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[在codepen上试一下](https://codepen.io/gaearon/pen/jrXYRR?editors=0011)

### 关键字－Keys

&emsp;关键字帮助react区别哪个item元素发生了变化，被加入，或者被删除。关键字应该赋值给数组中的元素，来让这个元素有一个稳定的标示。

```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

&emsp;选key最好的方式是使用一个字符串在兄弟元素中唯一的标示当前列表item元素。更常用的，你应该使用数据中id作为key：

```
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```

&emsp;当你没有针对渲染的item的稳定id时，最为最后的手段，你可以使用item的索引(index)作为关键字（key）:

```
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

&emsp;如果items可以重排序，我们不推荐使用索引作为关键字，因为那样会很慢。如果你感兴趣，可以读一下[深度解释为什么关键字是需要的](https://facebook.github.io/react/docs/reconciliation.html#recursing-on-children)这篇文章。

### 提取带关键字的组件-Extracting Components with Keys

关键字仅在数组环境下有意义。

例如，如果你提取一个listitem组件，你应该将关键字放在数组的<ListItem/>元素上，而不是放在ListItem里的<li>元素上。

例子：不正确的关键字使用

```
function ListItem(props) {
  const value = props.value;
  return (
    // Wrong! There is no need to specify the key here:
    <li key={value.toString()}>
      {value}
    </li>
  );
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Wrong! The key should have been specified here:
    <ListItem value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

例子：正确的关键字使用

```
function ListItem(props) {
  // Correct! There is no need to specify the key here:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // Correct! Key should be specified inside the array.
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

[codepen上试一下](https://codepen.io/rthor/pen/QKzJKG?editors=0010s)

### 关键字必须在兄弟姐妹中唯一-Keys Must Only Be Unique Among Siblings

数组中使用的关键字需要在兄弟姐妹中唯一。然而他们不需要全局唯一。我们在产生2个不同的数组时，可以使用同样的关键字：

```
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

[codepen上试一下](https://codepen.io/gaearon/pen/NRZYGN?editors=0010)

关键字对react来说是一种提示，但是他们不传递给组件。如果在组件中你需要同样的值，可以给他起一个别的名字，作为属性（prop）明确地传递出去：

```
const content = posts.map((post) =>
  <Post
    key={post.id}
    id={post.id}
    title={post.title} />
);
```

上面的例子，Post组件可以读取props.id，但不能读取props.key。

### 在jsx中嵌入map()-Embedding map() in JSX

上面的例子中我们声明了一个分离的listItems变量，并将其包含入JSX中：

```
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}
```

JSX允许在大括号中嵌入任何的表达式，所以我们可以包含map()结果：

```
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

[在codepen上试一下](https://codepen.io/gaearon/pen/BLvYrB?editors=0010)

有时候这个导致更清晰的代码，但是这种风格也能让人困惑。就像在javascript中，是否为了可读性抽取一个变量取决于你。记住，如果map()内太复杂，就该抽取一个组件。

## 表单-forms

在react中，HTML表单元素不同于其他DOM元素，因为表单元素保有一些内部状态。例如，下面的表单接受一个名字：

```
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

当这个用户提交表单时，这个表单有默认的html表单行为，即跳转到一个新页面。如果你在react中想要这个效果，也是可以的。但是在大部分场景下，使用js方法来处理表单的提交，访问用户输入，是更方便的。实现这个效果的标准方式是叫“控制的组件”的技术。

### 控制的组件-Controlled Components

在html中，表单元素如<input>,<textarea>和<select>通常维护他们自己的状态（state），并且基于用户输入更新这些状态。在react中，可变状态通常保存在组件的状态属性中，只用setState()更新。

我们可以使react状态成为“唯一的真理源”来结合两者。然后呈现表单的react组件在之后的用户输入中也控制表单的表现。一个值被react以这种方式控制的表单输入元素，被称为：控制的组件。

例如，如果我们想要之前的例子提交时，在日志打印出名字。我们可以将这个表单写为控制的组件：

```
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[在codepen上试一下](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

由于表格元素上设置了value属性，显示的值将会永远是this.state.value，使得react的状态是‘唯一的真理’。由于每次键盘点击都会触发handleChange，从而更新react状态，显示的值将会随用户输入而更新。

有了一个控制的组件，每一个状态的突变将会有一个对应的处理方法。这使得修改或验证用户的输入变得很直接。例如，如果我们想强制名字用大写字母编写，我们可以这样写handleChange:


```
handleChange(event) {
  this.setState({value:event.target.value.toUpperCase()});
}
```

###textarea标签
在Html中，一个<textarea>元素通过它的孩子定义它的内容：

```
<textarea>
  hello there,this is some text in a text area
</textarea>
```

在react中，一个<textarea>使用value属性来代替。这种方式，一个使用<textarea>的表单可以写的非常像一个使用单行input的表单：

```
class EssayForm extends React.Component {
  constructir(props) {
  super(props);
  this.state={
    value:'please write an essay'
  };
  this.handleChange=this.handleChange.bind(this);
  this.handleSubmit = this.handleSubmit.bind(this);
  }
  handleChange(event) {
  this.setState({value:event.target.value});
  }
  handleSubmit(event) {
  alert('an essay was submitted:'+this.state.value);
  event.preventDefault();
  }

  render() {
    return(
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange}/>
        </label>
        <input type="submit" value="Submit"/>
      </form>
      );
  }
}
```

注意this.state.value是在constructor中初始化的，所以text area开始的时候就有一些值。

###选择标签--the select tag
