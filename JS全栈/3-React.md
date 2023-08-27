## 一.JSX的语法和组件

## 1.JSX语法

JSX（JavaScript XML）是一种在React框架中用于描述用户界面的语法扩展。它允许您在JavaScript代码中编写类似于XML的标记，用于构建React组件的结构和外观。

```jsx
const name = "John";
const element = <h1>Hello, {name}!</h1>;
```

所谓JSX其实就是JavaScript对象，所以使用React和JSX的时候一定要经过编译的过程：

> JSX-->使用React构造组件，bable进行编译-->JavaScript对象-`ReactDOM.render()`-->DOM元素-->插入页面

## 2.Class组件

ES6的加入让JavaScript直接支持使用clas来定义一个类，react创建组件的方式就是使用的类的集成，`ES6class`是目前官方推荐的使用方式，它使用了ES6标准语法来构建。

```jsx
import React, { Component } from 'react';

class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  increment = () => {
    this.setState(prevState => ({
      count: prevState.count + 1
    }));
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}

export default Counter;
```

在上面的示例中，我们创建了一个名为 `Counter` 的类组件。类组件继承自 `React.Component`。它具有一个构造函数来初始化状态（state），并在类内部定义了一个 `increment` 方法来更新状态。`render` 方法用于返回组件的 JSX。

类组件的特点包括：

- 它们可以使用 `constructor` 来初始化状态和绑定方法。
- 它们可以使用生命周期方法（例如 `componentDidMount`、`componentDidUpdate` 等）来管理组件的生命周期事件。
- 它们可以使用 `this.props` 来访问传递给组件的属性。
- 它们可以拥有内部的状态（通过 `this.state` 访问），并且状态更新时会触发组件的重新渲染。

## 3.函数式组件

函数式组件（Functional Components）是 React 中定义组件的一种方式，它使用函数来表示组件。在 React 的早期版本中，类组件是主要的方式来创建组件，但随着时间的推移，函数式组件变得越来越流行，尤其是在引入了 React Hooks 后。

```jsx
import React from 'react';

function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;
```

在上面的示例中，我们创建了一个名为 `Greeting` 的函数式组件。它接受一个 `props` 对象作为参数，并返回一个包含 JSX 的 React 元素。

函数式组件的特点包括：

- 使用函数来定义组件，使代码更加简洁和易于阅读。
- 可以直接使用传递给组件的 `props`。
- 通常不需要像类组件那样使用 `constructor` 或生命周期方法。
- 使用函数组件时，可以充分利用 React Hooks 来处理状态、副作用等。

函数式组件的优势包括：

- 简洁性：函数式组件通常比类组件更简洁，因为它们不需要处理类的继承和状态初始化。
- 可测试性：由于函数式组件只是纯粹的 JavaScript 函数，因此它们通常更易于测试。
- 性能优化：函数式组件因为不涉及类的实例化和继承，可能在性能方面具有一些优势。

需要注意的是，虽然函数式组件是非常有用和强大的，但某些情况下，仍然可能需要使用类组件，尤其是在需要访问生命周期方法或在某些复杂情况下。然而，React 的开发团队推荐在新项目中使用函数式组件和 React Hooks 来编写组件。

***在 React 中，函数组件可以使用箭头函数的方式进行定义。**

**需要注意的是，与传统的函数定义相比，箭头函数具有更简洁的语法，特别适合用于定义简单的函数组件。在 React 中，函数组件和箭头函数成为了编写 UI 组件的常见方式，尤其是在使用 React Hooks 时。**

```jsx
import React from 'react';

const FunctionalComponent = (props) => {
  return (
    <div>
      <h1>Hello, {props.name}!</h1>
      <p>This is a functional component.</p>
    </div>
  );
};

export default FunctionalComponent;

```

## 4.组件的样式

### 4.1.内联样式

可以在组件的 JSX 中使用内联样式对象，将样式直接应用于元素。内联样式使用 JavaScript 对象表示，属性名采用驼峰命名法，值可以是字符串或数字。

```jsx
const styles = {
  color: 'blue',
  fontSize: '16px'
};

const MyComponent = () => {
  return <p style={styles}>This is a styled component.</p>;
};
```

### 4.2.使用外部 CSS 文件

可以将样式放在独立的外部 CSS 文件中，并通过在组件的 JSX 中添加 `className` 属性来引用这些样式。

```jsx
import './MyComponent.css'; // 导入外部样式文件

const MyComponent = () => {
  return <p className="styled-text">This is a styled component.</p>;
};

```

### 4.3.CSS模块

React 支持使用 CSS 模块来**局部化样式**，以避免全局样式冲突。每个 CSS 模块都可以为组件提供独立的样式。

```jsx
import styles from './MyComponent.module.css'; // 导入 CSS 模块

const MyComponent = () => {
  return <p className={styles.styledText}>This is a styled component.</p>;
};
```

### 4.4.CSS-in-JS 库 

有一些第三方库，如 Styled Components、Emotion 等，允许您在 JavaScript 中编写样式。这些库提供了更高级的样式管理和组件样式封装。

```jsx
import styled from 'styled-components';

const StyledText = styled.p`
  color: blue;
  font-size: 16px;
`;

const MyComponent = () => {
  return <StyledText>This is a styled component.</StyledText>;
};
```

## 5.事件的处理

### 5.1.绑定事件

### 5.2.事件handler的洗发

### 5.3.Event对象

## 6.Ref的应用

### 6.1.给标签设置ref="username"

### 6.2.给组件设置ref="username"

### 6.3.新的写法

