# react核心

重新再学一遍react，这次一定学好，多复习，多敲，为后面react生态学习夯实基础

------

## 创建项目

```node
npx create-react-app my_react
```

npx查找并执行，create-react-app创建react项目，my_react项目名

npm start 启动项目，默认端口3000

## 项目结构

### package.json

### src

```
src
│
├─ index.js
│
└─ app.js
```

#### index.js

index.js是整个项目的入口，项目从这里开始运行

```javascript
// 导入react必要的两个核心包
import React from 'react'
import ReactDOM from 'react-dom/client'

// 导入项目的根组件
import App from './App'

// 把App根组件渲染到一个id为root的dom节点上
const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
    <App />
)
```

#### app.js

app.js是项目根组件

```javascript
// App => index.js = >index.html(div#root)
function App() {
  return (
    <div className="App">
      this is app
    </div>
  )
}

export default App;
```

## JSX

### 概念

JSX是JavaScript和XML的缩写，表示在JS代码中编写HTML模板结构，他是React中编写UI模板的方式

优势：

1. HTML的声明式写法
2. JS的可编程能力

JSX通过babel转化为浏览器可以运行的JS

```jsx
//jsx

function test (){
 return (<div>this is app</div>)
}
test()
```

转化为

```javascript
//js

import { jsx as _jsx } from "react/jsx-runtime";
function test() {
  return /*#__PURE__*/_jsx("div", {
    children: "this is app"
  });
}
test();
```

### JSX中使用JS表达式

JSX中可以通过大括号语法{}识别JavaScript中的表达式，比如常见的变量，函数的调用，方法调用等

1. 使用引号传递字符串
2. 使用JavaScript变量
3. 函数调用和方法调用
4. 使用JavaScript对象

```jsx
const num = 100
const fn = (num) => {
  return num * 5
}
const obj = {
  name: 'jack'
}

function App() {
  return (
    <div className="App">
      {"字符串"}
      {num}
      {fn(num)}
      {new Date().getFullYear()}
      {obj.name}
      <div style={{color:'red'}}>this is red</div>
    </div>
  );
}

export default App;
```

### 列表渲染

1. map渲染哪个结构就return哪个结构
2. 循环的结构需要独一无二的key（key是react框架内部使用，提升更新性能的）
3. 再使用一层{}可以从循环对象中取值

```jsx
const list = [
  {
    "id": 1,
    "name": "Alice Johnson",
    "age": 28,
    "email": "alice.johnson@example.com"
  },
  {
    "id": 2,
    "name": "Bob Smith",
    "age": 34,
    "email": "bob.smith@example.com"
  },
  {
    "id": 3,
    "name": "Charlie Brown",
    "age": 22,
    "email": "charlie.brown@example.com"
  }
]


function App() {
  return (
    <div className="App">
      <ul>
        {list.map(item => <li key={item.id}>{item.name}-{item.age}-{item.email}</li>)}
      </ul>
    </div>
  );
}

export default App;
```

### 条件渲染

在React中，可以使用逻辑与运算(&&)、三元表达式(?:)实现基础的条件渲染

```jsx
const flag = true
const loading = true

function App() {
  return (
    <div className="App">
      {flag && <div>this is div</div>}
      {loading ? <span>Loading...</span> : <span>this is span</span>}
    </div>
  );
}

export default App;
```

### 事件绑定

on+事件名称 = { 事件处理程序 }，整体上遵循驼峰命名法

```jsx
const handleClick = ()=>{
  console.log("hello")
}

function App() {
  return (
    <div className="App">
      <button onClick={handleClick}>print</button>
    </div>
  );
}

export default App;
```

#### 使用事件对象参数

在事件回调函数中设置形参e

1. 传递事件对象

```jsx
const handleClick = (e)=>{
  console.log(e)
}

function App() {
  return (
    <div className="App">
      <button onClick={handleClick}>click me</button>
    </div>
  );
}

export default App;
```

2. 同时传递事件对象和自定义参数

```jsx
const handleClick = (e,str)=>{
  console.log(e,str)
}

function App() {
  return (
    <div className="App">
      <button onClick={(e)=>handleClick(e,"hello")}>click me</button>
    </div>
  );
}

export default App;
```

## 组件

一个组件就是用户界面的一部分，它可以有自己的逻辑和外观，组件之间可以互相嵌套，也可以被复用多次

### React组件

在React中，一个组件就是首字母大写的函数，内部存放了组件的逻辑和视图UI，渲染组件只需要把组件当成标签书写即可

```jsx
const handleClick = (e,str)=>{
  console.log(e,str)
}

function Button () {
  return (
    <button onClick={(e)=>handleClick(e,'hello')}>click me</button>
  )
}

function App() {
  return (
    <div className="App">
      <Button/>
      <Button></Button>
    </div>
  );
}

export default App;
```

### 组件基础样式控制

1. 行内样式
2. class类名控制，使用className进行样式绑定

使用 `className` 而不是 `class` 是因为 `class` 是 JavaScript 的保留字。为了避免与 JavaScript 关键字冲突，React 使用 `className` 来指定 CSS 类

```css
/* App.css */
.foo {
    color: blue;
    font-size: 50px;
}
```

```jsx
import './App.css'

function App() {
  return (
    <div className="App">
      <div style={{ fontSize: '20px', color: 'red' }}>this is red</div>
      <div className="foo">this is blue</div>
    </div>
  );
}

export default App;
```



## React Hook

### useState

useState是一个React Hook(函数)，它允许我们向组件添加一个状态变量，从而控制影响组件的渲染结果

本质：和普通JS变量不同的是，状态变量一旦发生变化组件的视图UI也会跟着变化(数据驱动视图)

```jsx
const [count,setCount] = useState(0)
```

1. useState是一个函数，返回值是一个数组
2. 数组中的第一个参数是状态变量，第二个参数是set函数用来修改状态变量
3. useState的参数作为状态变量的初始值

```jsx
import { useState } from 'react'

function Button () {
  const [i,setI]=useState(0)

  const handleClick = ()=>{
    setI(i+1)
  }
  return (
    <button onClick={handleClick}>click me:{i}</button>
  )
}

function App() {
  return (
    <div className="App">
      <Button/>
    </div>
  );
}

export default App;
```

在React中状态被认为是只读的，我们英国始终替换它而不是修改它，直接修改状态不能引发视图更新，甚至报错

#### 对象值的修改

```jsx
import { useState } from 'react'

function Button () {
  const [form,setForm]=useState({name:"jack"})

  const handleClick = ()=>{
    setForm({...form,name:'candy'})
  }
  return (
    <button onClick={handleClick}>click me:{form.name}</button>
  )
}

function App() {
  return (
    <div className="App">
      <Button/>
    </div>
  );
}

export default App;
```

### 







