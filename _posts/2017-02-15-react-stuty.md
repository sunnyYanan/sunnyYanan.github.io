---
layout: post
title:  react学习
categories: 前端开发
tag: datatable
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

