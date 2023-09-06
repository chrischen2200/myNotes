# 一.React库与React框架

1. React（也称为React.js或ReactJS）是一个自由及开放源代码的前端JavaScript工具，用于基于UI组件构建用户界面

1. React框架

   - 脚手架工具：create-vite，creat-react-app，Next.js ,Remix

   - 路由：react-router-dom

   - 状态管理：Redux，Mobx，Zustand
   - UI组件库：Ant Design，Material UI

   - Hooks库：ahooks，react-use

# 二.react模块与react-dom模块的作用

1. `react`模块：
   - `react`模块是React的核心部分，它定义了组件、虚拟DOM（Virtual DOM）和生命周期等核心概念。
   - 主要用于创建和定义React组件，包括函数组件和类组件。
   - 定义了组件的生命周期方法，允许你在组件的不同阶段执行代码，例如`componentDidMount`、`componentDidUpdate`等。
   - 提供了用于处理状态（state）和属性（props）的API，以及用于处理组件渲染的方法。
   - 不涉及与具体的渲染目标（如浏览器）交互，因此可以在不同的环境中使用，例如服务器端渲染（Server-side Rendering，SSR）。
2. `react-dom`模块：
   - `react-dom`模块是React库的一部分，但它专门用于与浏览器DOM交互的任务。
   - 主要用于将React组件渲染到浏览器中的真实DOM上。
   - 提供了一些与DOM元素交互的方法，例如`ReactDOM.render`用于初始化渲染，以及`ReactDOM.hydrate`用于在服务器端渲染之后在客户端进行重新渲染。
   - 也包括一些其他功能，如事件处理器的绑定，以确保React组件可以响应用户的交互。

# 三.JSX语法

JSX（JavaScript XML）是一种用于在JavaScript中编写类似XML或HTML的语法扩展，通常用于React应用程序中来定义UI组件的结构和外观。JSX使得在JavaScript中编写组件更加直观和可读，并且易于理解组件的嵌套结构。

以下是一些JSX语法的基本特点和示例：

1. **元素表示**：JSX允许你使用类似HTML的标签表示React组件。组件名称以大写字母开头，通常对应于一个自定义React组件或一个HTML标准元素。

   ```jsx
   const element = <h1>Hello, JSX!</h1>;
   ```

2. **属性**：你可以像HTML一样在JSX元素上添加属性，这些属性可以传递给组件。

   ```jsx
   const greeting = "Hello, JSX!";
   const element = <h1 className="greeting">{greeting}</h1>;
   ```

3. **插值表达式**：你可以在JSX中使用花括号 `{}` 来嵌入JavaScript表达式，这样你可以动态生成内容。

   ```jsx
   const name = "John";
   const greeting = <p>Hello, {name}!</p>;
   ```

4. **JSX中的注释**：你可以在JSX中使用注释，注释必须被包裹在花括号 `{}` 内。

   ```jsx
   const element = (
     <div>
       {/* This is a comment */}
       <p>Hello, JSX!</p>
     </div>
   );
   ```

5. **JSX中的事件处理**：你可以通过在JSX元素上添加事件处理函数来处理事件。

   ```jsx
   function handleClick() {
     alert("Button clicked!");
   }
   
   const button = <button onClick={handleClick}>Click me</button>;
   ```

6. **JSX中的条件渲染**：你可以使用JavaScript中的条件语句（如`if`或三元表达式）来根据条件渲染不同的内容。

   ```jsx
   const isLoggedIn = true;
   const element = isLoggedIn ? <p>Welcome, User!</p> : <p>Please log in</p>;
   ```

7. **JSX中的列表渲染**：你可以使用JavaScript的`map`函数来遍历数组并生成一组元素。

   ```jsx
   const numbers = [1, 2, 3, 4, 5];
   const listItems = numbers.map((number) => <li key={number}>{number}</li>);
   
   const list = <ul>{listItems}</ul>;
   ```

8. **JSX中的样式**：你可以使用行内样式对象来定义元素的样式。

   ```jsx
   const divStyle = {
     color: "blue",
     backgroundColor: "lightgray",
   };
   
   const element = <div style={divStyle}>Styled JSX Element</div>;
   ```

# 四.行间样式，局部样式，全局样式

在前端开发中，有不同的方式来应用样式（CSS）到网页元素中，包括行间样式、局部样式和全局样式。这些方法有不同的用途和适用场景：

1. **行间样式（Inline Styles）**：

   - 行间样式是将CSS样式直接应用到HTML元素的一种方式，通过元素的`style`属性来指定。
   - 这种方式允许你为特定元素指定样式，但它通常不是最佳实践，因为样式与HTML混合在一起，难以维护和重用。
   - 示例：

   ```jsx
   <div style="color: blue; font-size: 16px;">This is an inline-styled div.</div>
   ```

2. **局部样式（Scoped Styles）**：

   - 局部样式是指将CSS样式限定在某个特定的组件、模块或范围内，以避免全局样式冲突。
   - 在React应用中，你可以使用CSS模块、CSS-in-JS库或CSS预处理器来实现局部样式。
   - 这种方式提供了更好的模块化和可维护性，因为样式与组件相关联，不会泄漏到其他部分。
   - 示例（使用CSS模块）：

   ```jsx
   // Button.js
   import React from 'react';
   import styles from './Button.module.css';
   
   function Button() {
     return <button className={styles.button}>Click me</button>;
   }
   
   export default Button;
   ```

   ```jsx
   /* Button.module.css */
   .button {
     color: blue;
     font-size: 16px;
   }
   ```

3. **全局样式（Global Styles）**：

   - 全局样式是指应用于整个网站或应用程序的样式规则，通常位于全局CSS文件中。
   - 这种方式可以在整个项目中共享样式，但也容易导致样式冲突和难以维护。
   - 使用全局样式时，通常需要遵循一致的命名约定和组织结构，以减少冲突的可能性。
   - 示例：

   ```jsx
   /* global.css */
   body {
     font-family: Arial, sans-serif;
   }
   
   .container {
     max-width: 1200px;
     margin: 0 auto;
   }
   
   /* Other global styles... */
   ```

# 五.classnames模块

`classnames`是一个常用于React应用中的JavaScript库，它用于动态生成HTML元素的`class`属性。这个库使得在React组件中更容易地处理条件性的类名，特别是在需要根据组件的不同状态或属性来设置类名时非常有用。通过使用`classnames`库，你可以更简洁地管理多个类名的组合。

以下是使用`classnames`库的基本示例和用法：

1. **安装和导入**： 首先，确保你的项目中已经安装了`classnames`库。你可以使用npm或yarn进行安装：

   ```bash
   npm install classnames
   # 或
   yarn add classnames
   ```

   然后，在React组件文件中导入`classnames`库：

   ```js
   import classNames from 'classnames';
   ```

2. **基本用法**： 使用`classnames`函数，你可以将多个类名作为参数传递，它将返回一个字符串，其中包含了合并的类名：

   ```js
   const buttonClass = classNames('button', 'primary', { 'active': isActive });
   ```

   在上面的示例中，如果`isActive`为`true`，则`buttonClass`将包含三个类名：'button'、'primary'和'active'；如果`isActive`为`false`，则只包含两个类名：'button'和'primary'。

3. **条件性类名**： `classnames`还可以用于根据条件添加或删除类名，这在处理不同状态的组件时非常有用：

   ```js
   const buttonClass = classNames('button', {
     'primary': isPrimary,
     'active': isActive,
     'disabled': isDisabled,
   });
   ```

   在上面的示例中，类名'primary'、'active'和'disabled'将根据`isPrimary`、`isActive`和`isDisabled`的值进行条件性添加。

4. **使用在React组件中**： 在React组件中，你可以将生成的`classnames`字符串分配给`class`属性：

   ```jsx
   <button className={buttonClass}>Click me</button>
   ```

   这将应用生成的类名到按钮元素上。

总的来说，`classnames`库是一个有用的工具，可以帮助你更方便地管理和处理类名，特别是在React应用中，当需要根据组件状态或属性动态生成类名时。这有助于使你的代码更具可读性和可维护性。

# 六.React元素添加事件

在React中，你可以将事件处理程序附加到元素，使其响应各种事件，如点击、鼠标移入、键盘输入等。以下是在React中给元素添加事件处理程序的基本方法：

1. **JSX中添加事件**： 在JSX中，你可以使用类似HTML的事件属性来添加事件处理程序。例如，要为按钮元素添加点击事件处理程序，可以使用`onClick`属性：

   ```jsx
   function MyComponent() {
     const handleClick = () => {
       alert('Button clicked!');
     };
   
     return (
       <button onClick={handleClick}>Click me</button>
     );
   }
   ```

   在上述示例中，`handleClick`函数将在按钮被点击时执行。

2. **传递参数给事件处理程序**： 如果需要将自定义参数传递给事件处理程序，你可以使用箭头函数或`.bind()`方法：

   ```jsx
   function MyComponent() {
     const handleCustomClick = (param) => {
       alert(`Button clicked with param: ${param}`);
     };
   
     return (
       <button onClick={() => handleCustomClick('customParam')}>Click me</button>
     );
   }
   ```

   或者使用`.bind()`：

   ```jsx
   function MyComponent() {
     const handleCustomClick = (param) => {
       alert(`Button clicked with param: ${param}`);
     };
   
     return (
       <button onClick={handleCustomClick.bind(this, 'customParam')}>Click me</button>
     );
   }
   ```

3. **事件对象**： React事件处理程序通常会接收一个事件对象作为参数，你可以使用这个事件对象来获取有关事件的信息：

   ```jsx
   function handleMouseEnter(event) {
     alert(`Mouse entered at X: ${event.clientX}, Y: ${event.clientY}`);
   }
   
   return (
     <div onMouseEnter={handleMouseEnter}>Hover me</div>
   );
   ```

4. **阻止默认行为**： 有些事件会触发默认行为，例如点击链接导航到新页面。你可以使用`preventDefault`方法来阻止这些默认行为：

   ```jsx
   function handleLinkClick(event) {
     event.preventDefault();
     // 添加自定义逻辑
   }
   
   return (
     <a href="https://example.com" onClick={handleLinkClick}>Click me</a>
   );
   ```

这些示例演示了如何在React中给元素添加事件处理程序。使用React的事件系统，你可以方便地管理各种事件，并根据需要执行自定义逻辑。

# 七.条件渲染

在React中，条件渲染是指根据特定条件来决定是否渲染组件或元素的一种技术。这使得你可以根据应用程序的状态或用户输入来动态显示或隐藏内容。以下是在React中进行条件渲染的常见方法：

1. **条件运算符（Ternary Operator）**： 使用条件运算符（三元运算符）可以根据条件渲染不同的内容。

   ```jsx
   function MyComponent({ isLoggedIn }) {
     return (
       <div>
         {isLoggedIn ? <p>Welcome, User!</p> : <p>Please log in</p>}
       </div>
     );
   }
   ```

   在上面的示例中，根据`isLoggedIn`的值，渲染不同的欢迎消息。

2. **逻辑与运算符（Logical && Operator）**： 你可以使用逻辑与运算符来根据条件渲染组件或元素。

   ```jsx
   function MyComponent({ hasData, data }) {
     return (
       <div>
         {hasData && <p>Data: {data}</p>}
       </div>
     );
   }
   ```

   在上面的示例中，只有当`hasData`为`true`时，才会渲染包含数据的元素。

3. **条件渲染函数**： 你可以创建一个函数来根据条件返回不同的内容。

   ```jsx
   function renderContent(isLoggedIn) {
     if (isLoggedIn) {
       return <p>Welcome, User!</p>;
     } else {
       return <p>Please log in</p>;
     }
   }
   
   function MyComponent({ isLoggedIn }) {
     return (
       <div>
         {renderContent(isLoggedIn)}
       </div>
     );
   }
   ```

   这种方法允许你将条件渲染逻辑封装到一个函数中，以提高可读性。

4. **条件渲染组件**： 你可以创建多个组件，并根据条件选择性地渲染它们。

   ```jsx
   function WelcomeMessage() {
     return <p>Welcome, User!</p>;
   }
   
   function LoginMessage() {
     return <p>Please log in</p>;
   }
   
   function MyComponent({ isLoggedIn }) {
     return (
       <div>
         {isLoggedIn ? <WelcomeMessage /> : <LoginMessage />}
       </div>
     );
   }
   ```

   在上述示例中，根据`isLoggedIn`的值，选择性地渲染不同的组件。

这些方法可以根据你的需求选择，并根据应用程序的状态和用户行为来动态渲染内容。条件渲染是构建动态、交互性React应用的重要组成部分。

# 八.组件通信

在React中，组件通信是指不同组件之间传递数据、状态或消息的方式。React应用程序通常由多个组件组成，这些组件之间需要相互交互和通信。以下是React中常见的组件通信方法：

1. **Props传递**： 最简单的组件通信方式是通过`props`（属性）来传递数据。父组件可以将数据作为`props`传递给子组件，子组件可以读取这些`props`并在其内部使用。

   ```jsx
   // 父组件
   function ParentComponent() {
     const data = "Hello from Parent";
     return <ChildComponent message={data} />;
   }
   
   // 子组件
   function ChildComponent(props) {
     return <p>{props.message}</p>;
   }
   ```

2. **回调函数**： 父组件可以通过`props`将回调函数传递给子组件，子组件可以调用这些回调函数来通知父组件或传递数据。

   ```jsx
   // 父组件
   function ParentComponent() {
     const handleChildClick = (data) => {
       alert(`Data from Child: ${data}`);
     };
   
     return <ChildComponent onClick={handleChildClick} />;
   }
   
   // 子组件
   function ChildComponent(props) {
     const sendDataToParent = () => {
       props.onClick("Hello from Child");
     };
   
     return <button onClick={sendDataToParent}>Send Data</button>;
   }
   ```

3. **Context API**： Context API允许在React组件之间共享数据，而不必通过`props`逐层传递。这对于在应用程序中的多个组件之间共享全局状态非常有用。

   ```jsx
   // 创建一个上下文
   const MyContext = React.createContext();
   
   // 父组件
   function ParentComponent() {
     return (
       <MyContext.Provider value="Hello from Context">
         <ChildComponent />
       </MyContext.Provider>
     );
   }
   
   // 子组件
   function ChildComponent() {
     const data = React.useContext(MyContext);
     return <p>{data}</p>;
   }
   ```

4. **Redux或其他状态管理库**： 对于大型应用程序，使用状态管理库（如Redux）可以更好地管理应用程序的状态和数据流。这些库提供了一个中央存储，可以在组件之间共享和调度数据。

5. **事件总线**： 你可以使用事件总线模式来实现组件之间的通信。这涉及到在应用程序中创建一个事件总线对象，允许组件发布和订阅事件。但请注意，这种方法通常不推荐，除非你的应用程序非常复杂且没有更好的方式来管理通信。

# 八.状态不可变原则

1. **状态不可变性**： 在React中，状态是不可变的，这意味着一旦创建了状态，就不能直接修改其值。你应该创建一个新的状态对象来表示状态的变化。

2. **使用setState方法**： 要更新组件的状态，应该使用`setState`方法（在类组件中）或`useState`钩子（在函数组件中）。这些方法会在内部处理状态的更新，确保了不可变性原则。

   ```
   jsxCopy code
   // 类组件中更新状态
   this.setState({ count: this.state.count + 1 });
   
   // 函数组件中更新状态
   setCount(count + 1);
   ```

   上述示例中，我们创建了一个新的状态对象，而不是直接修改现有状态。

3. **展开操作符（Spread Operator）**： 在React中，通常使用展开操作符来创建新的状态对象，以便更新部分状态而不改变其他部分。

   ```
   jsxCopy code
   // 使用展开操作符来更新状态
   this.setState({ ...this.state, count: this.state.count + 1 });
   ```

   这会创建一个包含旧状态属性的新状态对象，并仅更改`count`属性。

4. **不要直接修改状态对象**： 直接修改状态对象是不安全的，并且可能导致不可预测的结果。React依赖于状态的不可变性来检测状态的变化并触发重新渲染。

   ```
   jsxCopy code
   // 不要这样做！
   this.state.count = this.state.count + 1;
   ```

5. **性能优化**： 不可变性也有助于React进行性能优化。React可以比较新旧状态对象来确定是否需要重新渲染组件，从而提高性能。

6. **深拷贝和浅拷贝**： 注意，在某些情况下，展开操作符会执行浅拷贝，这意味着嵌套的对象或数组可能仍然引用相同的内存地址。如果需要进行深度拷贝以确保不可变性，可以使用辅助函数或库，如`lodash`的`cloneDeep`。

   总之，React的状态不可变性原则是确保状态管理的重要原则之一。通过遵循这个原则，你可以更容易地追踪和调试状态的变化，并提高React应用程序的可维护性和性能。

# 九.immer模块

`immer` 是一个用于处理不可变性的JavaScript库，它可以帮助你以一种更直观、更易于理解的方式来修改不可变数据。虽然 `immer` 不是 React 的官方库，但它与 React 配合使用非常流行，尤其是在处理复杂的状态管理或更新深层嵌套对象时非常有用。

以下是关于 `immer` 的一些重要概念和用法：

1. **安装 immer**： 你可以使用 npm 或 yarn 来安装 `immer`：

   ```
   bashCopy code
   npm install immer
   # 或
   yarn add immer
   ```

2. **基本使用**： `immer` 提供了一个 `produce` 函数，你可以用它来修改不可变数据结构，而无需手动复制对象。下面是一个基本的示例：

   ```
   javascriptCopy code
   import produce from 'immer';
   
   const originalState = {
     value: 1,
     nested: {
       nestedValue: 2,
     },
   };
   
   const newState = produce(originalState, (draft) => {
     draft.value = 3;
     draft.nested.nestedValue = 4;
   });
   
   console.log(originalState); // 不会被修改
   console.log(newState); // 新状态
   ```

   `produce` 函数接受原始状态和一个函数，该函数接受一个可修改的 "draft" 对象，你可以在其中进行任何修改。`immer` 会确保只有在修改 draft 对象时才会创建新的状态。

3. **嵌套对象**： `immer` 非常适合修改嵌套的对象，因为它会智能地处理深层嵌套的不可变性。

4. **返回新的状态**： `immer` 的 `produce` 函数会返回新的状态。你需要将这个新的状态分配给你的 React 组件的状态或 Redux 状态来触发重新渲染或状态管理的更新。

5. **避免副作用**： `immer` 鼓励不可变性，但仍然允许你在 `produce` 函数中进行修改。然而，请注意，修改应该在 `produce` 函数内部完成，避免在函数外部引入副作用。

6. **在 React 中使用**： 在 React 应用中，你可以将 `immer` 与组件的状态管理或 Redux 等状态管理库一起使用，以更轻松地管理和更新状态。通过 `immer`，你可以以更直观的方式编写状态更新逻辑，而不必担心破坏不可变性。

总之，`immer` 是一个强大的 JavaScript 库，用于简化不可变性数据的处理。它特别适用于 React 应用程序中的状态管理和数据更新，可以帮助你编写更清晰、更易维护的代码。

# 十.状态提升

状态提升是一种在React中用于管理组件之间共享状态的模式。这个模式的核心思想是将状态从子组件移动到它们的共同父组件中，以使多个子组件可以访问和修改相同的状态。这有助于保持应用程序的状态一致性并简化数据流。

以下是有关状态提升的关键概念和示例：

1. **为什么使用状态提升**：

   - **共享数据**：当多个组件需要访问和共享相同的数据时，将数据提升到共同的父组件中是有意义的。
   - **同步状态**：通过将状态提升，可以确保多个子组件之间的状态保持同步，以防止数据不一致。
   - **简化数据流**：避免通过组件树传递回调函数或使用其他复杂的方法来共享状态，可以简化数据流。

2. **示例**： 假设你有一个包含两个子组件的父组件，其中一个子组件用于显示数据，另一个子组件用于修改数据。你可以通过将数据状态提升到父组件来实现这种共享状态的模式。

   ```jsx
   function ParentComponent() {
     const [data, setData] = useState('Initial Data');
   
     const handleDataChange = (newData) => {
       setData(newData);
     };
   
     return (
       <div>
         <ChildDisplay data={data} />
         <ChildModify onDataChange={handleDataChange} />
       </div>
     );
   }
   
   function ChildDisplay({ data }) {
     return <p>Data: {data}</p>;
   }
   
   function ChildModify({ onDataChange }) {
     const handleChange = () => {
       onDataChange('Updated Data');
     };
   
     return (
       <div>
         <button onClick={handleChange}>Change Data</button>
       </div>
     );
   }
   ```

   在这个示例中，`ParentComponent` 管理了 `data` 状态，并将其传递给两个子组件 `ChildDisplay` 和 `ChildModify`。`ChildModify` 子组件通过回调函数 `onDataChange` 来修改 `data` 状态，以保持状态的同步。

3. **状态提升的注意事项**：

   - 状态提升可能会导致组件层次结构变得更深。要确保提升状态是有意义的，不要仅仅为了共享状态而提升。
   - 在某些情况下，使用状态管理库（如Redux或Mobx）可能更合适，特别是在应用程序的状态管理变得复杂时。

状态提升是React中一种有用的模式，它有助于组织和管理组件之间的共享状态，并确保数据一致性。根据应用程序的需求，你可以灵活选择是否采用状态提升或使用状态管理库来管理状态。

# 十一.状态的重置处理问题

1. 当组件被销毁时，所对应的状态也会被重置

   当组件卸载时，React会自动清除其状态以释放内存并确保不会出现悬挂的引用。这意味着在组件被重新渲染或重新挂载之前，状态将被重置为初始值。

2. 当组件位置没有发生改变时，状态是会被保留的

   在React中，如果组件的位置没有发生改变（即组件没有被卸载和重新挂载），组件的状态通常会被保留。React会尽力保持组件的状态不变，以提高性能并确保组件可以正确地保存和恢复其状态。

   以下是一些情况下组件状态会被保留的示例：

   1. **组件的挂载和卸载**： 当组件挂载后，其状态会被初始化并保留在内存中。如果组件在后续渲染中没有被卸载，状态将继续存在，除非你在组件内部显式修改或重置状态。
   2. **React的虚拟DOM比较**： React使用虚拟DOM来比较前后两次渲染的状态和UI差异。如果组件在两次渲染之间没有被卸载，React会尽量保持其状态不变，只更新需要更新的部分。
   3. **合适的生命周期方法**： 组件的生命周期方法，如`componentDidUpdate`，可以用于在组件更新时进行状态的处理和更新。这可以让你控制状态的变化，并确保不会不必要地重置状态。

   尽管React通常会尽量保持组件状态不变，但也需要注意以下情况：

   - 如果组件被卸载，然后重新挂载，状态将被重置为初始值，除非你在重新挂载后手动恢复状态。
   - 如果组件使用了`key`属性，而`key`属性的值发生变化，React将认为这是一个不同的组件实例，状态将被重置。
   - 在某些情况下，如果你使用了不纯的函数或副作用，可能会导致状态不一致。在这种情况下，你需要小心处理状态的保留和更新。

# 十二.计算变量

计算变量通常是指在组件的渲染方法中使用JavaScript表达式或函数来生成动态的内容，这些内容可以在渲染过程中显示在用户界面上。计算变量使你能够根据组件的状态或其他数据来动态生成内容，以提供更灵活和交互性的用户界面。

计算变量可以采用以下方式创建：

1. **在`render`方法中使用JavaScript表达式**： 你可以在类组件的`render`方法中使用JavaScript表达式来计算变量的值，然后将这些值嵌入JSX中。例如：

   ```jsx
   class MyComponent extends React.Component {
     render() {
       const firstName = 'John';
       const lastName = 'Doe';
       const fullName = `${firstName} ${lastName}`;
   
       return (
         <div>
           <p>First Name: {firstName}</p>
           <p>Last Name: {lastName}</p>
           <p>Full Name: {fullName}</p>
         </div>
       );
     }
   }
   ```

2. **在函数组件中使用JavaScript表达式**： 在函数组件中，你可以在函数的主体中计算变量并将它们返回到JSX中。例如：

   ```jsx
   import React from 'react';
   
   function MyComponent() {
     const firstName = 'John';
     const lastName = 'Doe';
     const fullName = `${firstName} ${lastName}`;
   
     return (
       <div>
         <p>First Name: {firstName}</p>
         <p>Last Name: {lastName}</p>
         <p>Full Name: {fullName}</p>
       </div>
     );
   }
   ```

3. **在函数组件中使用计算属性**： 使用`useState`钩子来管理组件的状态，可以在函数组件中使用计算属性。计算属性允许你根据状态值计算其他变量的值，以在渲染中使用。

   ```jsx
   import React, { useState } from 'react';
   
   function MyComponent() {
     const [count, setCount] = useState(0);
     const doubleCount = count * 2;
   
     return (
       <div>
         <p>Count: {count}</p>
         <p>Double Count: {doubleCount}</p>
         <button onClick={() => setCount(count + 1)}>Increment Count</button>
       </div>
     );
   }
   ```

计算变量使你能够以动态的方式生成和展示数据，以响应组件的状态变化或其他外部因素。这可以帮助你构建更具交互性和动态性的React组件。

# 十三.受控组件与非受控组件

1. 通过props控制的组件称为受控组件；而通过state控制的组件称为非受控组件
2. React表单内置了受控组件的行为
   - value+onChange
   - checked+onChange

# 十四.Hooks

React中的Hooks是一些内置的函数，它们允许你在函数组件中添加状态管理、副作用、生命周期等功能，使函数组件具有类似于类组件的能力。以下是一些常用的React Hooks：

1. **useState**： `useState` Hook 允许你在函数组件中添加状态管理。它返回一个包含当前状态值和更新状态值的数组。你可以使用它来创建和更新组件的状态。

   ```jsx
   import React, { useState } from 'react';
   
   function Counter() {
     const [count, setCount] = useState(0);
   
     const increment = () => {
       setCount(count + 1);
     };
   
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={increment}>Increment</button>
       </div>
     );
   }
   ```

2. **useRef**： React 中的一个 Hook，用于创建可变的引用对象。它经常用于访问组件中的 DOM 元素或在组件之间共享可变数据。**`useRef` 的一个主要特点是，当你更新引用对象的值时，不会触发组件重新渲染。**

   - **保存可变数据**：`useRef` 还可以用于存储组件中的可变数据，而不会触发重新渲染。这对于跟踪组件之间的状态或引用非受控组件非常有用。

     ```jsx
     import React, { useRef, useState } from 'react';
     
     function MyComponent() {
       const counter = useRef(0);
       const [count, setCount] = useState(0);
     
       const handleClick = () => {
         counter.current += 1;
         setCount(counter.current);
       };
     
       return (
         <div>
           <p>Counter: {count}</p>
           <button onClick={handleClick}>Increment</button>
         </div>
       );
     }
     ```

     在这个示例中，我们使用 `useRef` 创建了一个 `counter` 引用，并初始化为 `0`。每次点击按钮时，我们更新 `counter.current`，而不会触发组件重新渲染，从而实现了一个独立的计数器。

   - **访问 DOM 元素**：你可以使用 `useRef` 来获取对 DOM 元素的引用，以便在需要时操作 DOM。

     ```jsx
     import React, { useRef, useEffect } from 'react';
     
     function MyComponent() {
       const myRef = useRef();
     
       useEffect(() => {
         // 使用 myRef.current 来访问 DOM 元素
         myRef.current.focus();
       }, []);
     
       return <input ref={myRef} />;
     }
     ```

     在上述示例中，我们创建了一个 `myRef` 引用，并将其传递给 `input` 元素的 `ref` 属性。然后，我们可以使用 `myRef.current` 来访问该输入框元素，以便在组件挂载后聚焦到它。

   - **forwardRef**： `ref` 和 `forwardRef` 可以让你在React中更灵活地操作子组件中的DOM元素或组件实例。

     ```jsx
     import {forwardRef, useRef} from 'react'
     
     // eslint-disable-next-line react/display-name
     const MyInput = forwardRef(( props,ref) => {
         return (
             <div>
                 <input type="text" ref={ref}/>
             </div>
         )
     })
     
     const App = () => {
         const myRef = useRef(null)
         const handleClick = () => {
             myRef.current.style.backgroundColor = 'red'
         }
         return (
             <div>
                 <button onClick={handleClick}>click me</button>
                 <MyInput  ref={myRef}/>
             </div>
         )
     
     }
     export default App
     ```

3. **useImperativeHandle**：是 React 中的一个 Hook，用于自定义一个组件的暴露给父组件的实例值。通常，React 推荐使用 props 来进行组件之间的通信，但有时候你可能需要直接访问子组件中的方法或属性。`useImperativeHandle` 允许你在子组件中定义一些特定的方法或属性，然后将它们暴露给父组件。

   ```jsx
   import React, { useRef, useImperativeHandle, forwardRef } from 'react';
   
   // 子组件
   const ChildComponent = forwardRef((props, ref) => {
     const childInputRef = useRef();
   
     // 使用 useImperativeHandle 定义暴露给父组件的方法或属性
     useImperativeHandle(ref, () => ({
       focusChild: () => {
         childInputRef.current.focus();
       },
       // 还可以暴露其他方法或属性
     }));
   
     return <input ref={childInputRef} />;
   });
   
   // 父组件
   function ParentComponent() {
     const childRef = useRef();
   
     const handleClick = () => {
       // 调用子组件的 focusChild 方法
       childRef.current.focusChild();
     };
   
     return (
       <div>
         <ChildComponent ref={childRef} />
         <button onClick={handleClick}>Focus Child</button>
       </div>
     );
   }
   
   export default ParentComponent;
   ```

   在这个示例中，`useImperativeHandle` 被用来定义了一个名为 `focusChild` 的方法，并将其暴露给父组件。这个方法的作用是让子组件中的输入框获取焦点。

   在父组件中，我们使用 `useRef` 创建了一个 `childRef`，并将其传递给子组件 `ChildComponent` 的 `ref` 属性。然后，我们可以通过调用 `childRef.current.focusChild()` 来调用子组件中的 `focusChild` 方法，从而聚焦子组件的输入框。

   总之，`useImperativeHandle` 可以让你自定义组件的暴露给父组件的方法或属性，从而实现更灵活的组件通信和操作。

4. **useEffect**： `useEffect` Hook 用于处理副作用操作，如数据获取、DOM操作、订阅等。<font color='red'>**它接受一个函数作为参数，该函数在JSX组件初始渲染、更新渲染、卸载后执行。**</font>

   副作用的概念：

   - 函数在执行过程中对外部造成的影响称之为副作用，例如：AJax调用，DOM操作，与外部系统同步等
   - 在React组件中，事件操作是可以处理副作用的，但有时候需要初始化处理副作用，那么就需要useEffect钩子

   ```jsx
   import React, { useState, useEffect } from 'react';
   
   function Example() {
     const [data, setData] = useState([]);
   
     useEffect(() => {
       // 执行副作用操作，如数据获取
       fetch('https://api.example.com/data')
         .then(response => response.json())
         .then(data => setData(data));
     }, []); // 传递一个空数组以防止多次执行副作用
   }
   //如果不传递第二个参数（数组），useEffect会在每次组件渲染时都触发。这意味着每次组件更新时都会执行副作用代码。
   //如果传递一个空数组作为第二个参数，useEffect只会在组件挂载时执行一次，类似于componentDidMount生命周期。
   //如果传递一个包含依赖项的数组作为第二个参数，useEffect会在依赖项发生变化时执行。这可以用于控制副作用代码在特定条件下执行。
   //内部是通过Object.is()來判定是否改变
   ```

   ```jsx
   //useEffect的清理工作
   //1.卸载组件的时候 2.下一次更新前，清理当前作用域
   useEffect(() => {
     // 这里的代码在组件挂载后执行
   
     return () => {
       // 这里的代码在组件卸载时执行
     };
   }, []);
   ```

5. **useLayoutEffect**：它类似于 `useEffect`，用于处理副作用操作，<font color='red'>**但有一个重要的区别：`useLayoutEffect` 会在浏览器布局（layout）更新之前同步触发，而 `useEffect` 则是在布局更新之后异步触发。**</font>

   具体来说，`useLayoutEffect` 与 `useEffect` 的使用方式相似，都接受两个参数：一个函数（用于执行副作用操作）和一个依赖数组。<font color='red'>**但不同之处在于 `useLayoutEffect` 的副作用代码会在浏览器布局更新之前立即执行，而 `useEffect` 的副作用代码会在浏览器布局更新之后执行。**</font>

   这个差异对于某些场景非常重要，特别是在需要获取 DOM 元素的位置和尺寸信息以及进行同步 DOM 操作时。通常情况下，你应该首选使用 `useEffect`，因为它的异步性能更好，不会阻塞页面的渲染。只有在确切需要在布局更新之前执行某些操作时，才应考虑使用 `useLayoutEffect`。

   下面是一个使用 `useLayoutEffect` 的简单示例：

   ```jsx
   import React, { useLayoutEffect, useState, useRef } from 'react';
   
   function MyComponent() {
     const [width, setWidth] = useState(0);
     const divRef = useRef(null);
   
     useLayoutEffect(() => {
       // 这里的代码会在布局更新之前立即执行
       const newWidth = divRef.current.clientWidth;
       setWidth(newWidth);
     }, []);
   
     return (
       <div ref={divRef}>
         <p>宽度：{width}px</p>
       </div>
     );
   }
   ```

   在这个示例中，`useLayoutEffect` 用于获取 `divRef` 的宽度信息，并将其保存在 `width` 状态中，这个操作会在布局更新之前立即执行，以确保获取到准确的宽度信息。

   总之，`useLayoutEffect` 是一个用于处理同步布局操作的 React Hook，它会在浏览器布局更新之前触发，但要谨慎使用，避免影响性能。通常情况下，应该首选使用 `useEffect`。

6. **useInsertionEffect**：它与useLayoutEffect Hook非常相似，但它不能访问DOM节点的引用。这意味着它只能用于插入样式规则。它的主要用途是插入全局DOM节点，如<style> 等。

   ```jsx
   import { useLayoutEffect } from "react";
   import { useEffect, useInsertionEffect, useRef } from "react";
    
   function getStyleForRule(rule) {
     let style = document.createElement("style");
     style.innerHTML = rule;
     return style;
   }
   function useCSS(rule) {
     useInsertionEffect(() => {
       console.log("每次渲染前")
       // 插入style标签
       document.head.appendChild(getStyleForRule(rule));
     });
     return rule;
   }
   export default function Component() {
     useLayoutEffect(() => {
       console.log("每次渲染后的同步班")
     })
     useEffect(() => {
       console.log("每次渲染后")
     })
     let className = useCSS(`.red{
       background-color:red;
   }`);
     return <div className={"red"}>哈哈哈</div>;
   }
   ```

7. **useEffect && useLayoutEffect && useInsertionEffect**：

   ![6d5874fdb68e4d4c99a96e2c400e1c8c](https://i.postimg.cc/vTLTkkg3/6d5874fdb68e4d4c99a96e2c400e1c8c.png)

8. **useReducer**：用于在函数组件中进行状态管理。它通常用于处理具有复杂状态逻辑的组件，以及在状态更新时触发副作用操作。

   `useReducer` 接受两个参数：一个是 reducer 函数，另一个是初始状态。它返回一个包含当前状态和 dispatch 函数的数组。

   下面是一个基本的示例，演示如何使用 `useReducer`：

   ```jsx
   import React, { useReducer } from 'react';
   
   // 定义一个 reducer 函数，接受当前状态和一个 action，并返回新的状态
   function counterReducer(state, action) {
     switch (action.type) {
       case 'INCREMENT':
         return { count: state.count + 1 };
       case 'DECREMENT':
         return { count: state.count - 1 };
       default:
         return state;
     }
   }
   
   function Counter() {
     // 使用 useReducer 定义状态和 dispatch 函数
     const [state, dispatch] = useReducer(counterReducer, { count: 0 });
   
     return (
       <div>
         <p>Count: {state.count}</p>
         <button onClick={() => dispatch({ type: 'INCREMENT' })}>增加</button>
         <button onClick={() => dispatch({ type: 'DECREMENT' })}>减少</button>
       </div>
     );
   }
   
   export default Counter;
   ```

   在这个示例中，我们定义了一个 `counterReducer` 函数，用于处理状态的更新。`useReducer` 的第一个参数是这个 reducer 函数，第二个参数是初始状态。`useReducer` 返回一个包含 `state` 和 `dispatch` 的数组，其中 `state` 包含了当前的状态，`dispatch` 是一个用于派发操作的函数。

   通过调用 `dispatch` 函数并传递一个操作类型（在这个例子中是 `'INCREMENT'` 或 `'DECREMENT'`），我们可以触发状态的更新。Reducer 函数将根据操作类型返回新的状态。

   `useReducer` 的优点之一是它适用于处理较复杂的状态逻辑，可以将相关的操作和状态更新逻辑集中在一个 reducer 函数中，提高了代码的可维护性。这在管理组件的本地状态时非常有用。

9. **useContext**：是 React 提供的一个 Hook，用于在函数组件中访问和使用 React 的上下文（Context）。上下文是 React 中一种用于跨组件传递数据的机制，它允许你将数据传递给组件树中的多个组件，而无需手动一级一级地将数据传递下去。

   `useContext` 接受一个上下文对象作为参数，并返回上下文提供的值。通常，你需要在某个组件树的更上层使用 `Context.Provider` 来提供上下文的值，然后在子组件中使用 `useContext` 来获取这些值。

   以下是一个简单的示例，演示如何使用 `useContext`：

   ```jsx
   import React, { createContext, useContext } from 'react';
   
   // 创建一个上下文对象
   const MyContext = createContext();
   
   // 提供上下文的值，通常在某个更高级的组件中
   function App() {
     const data = "这是上下文的数据";
     
     return (
       <MyContext.Provider value={data}>
         <ChildComponent />
       </MyContext.Provider>
     );
   }
   
   // 子组件中使用 useContext 获取上下文的值
   function ChildComponent() {
     const contextData = useContext(MyContext);
   
     return (
       <div>
         <p>上下文的值：{contextData}</p>
       </div>
     );
   }
   
   export default App;
   ```

   在这个示例中，我们首先创建了一个上下文对象 `MyContext`，然后在 `App` 组件中使用 `MyContext.Provider` 提供了上下文的值，这个值是一个字符串 "这是上下文的数据"。接下来，在 `ChildComponent` 中使用 `useContext(MyContext)` 获取了上下文的值，并渲染到了页面上。

   通过使用 `useContext`，我们可以避免了一级一级地将数据传递给每个子组件，而是直接从上下文中获取需要的数据，使组件之间的数据传递更加便捷和高效。上下文还常用于处理全局的应用程序状态，例如用户认证状态、主题等。在这个示例中，我们首先创建了一个上下文对象 `MyContext`，然后在 `App` 组件中使用 `MyContext.Provider` 提供了上下文的值，这个值是一个字符串 "这是上下文的数据"。接下来，在 `ChildComponent` 中使用 `useContext(MyContext)` 获取了上下文的值，并渲染到了页面上。

   通过使用 `useContext`，我们可以避免了一级一级地将数据传递给每个子组件，而是直接从上下文中获取需要的数据，使组件之间的数据传递更加便捷和高效。上下文还常用于处理全局的应用程序状态，例如用户认证状态、主题等。

10. **useReducer**： `useReducer` Hook 允许你管理复杂的状态逻辑。它接受一个reducer函数和初始状态，并返回当前状态和dispatch函数。通常用于替代`useState`，尤其在状态变更逻辑复杂的情况下。

   ```jsx
   import React, { useReducer } from 'react';
   
   function reducer(state, action) {
     switch (action.type) {
       case 'INCREMENT':
         return { count: state.count + 1 };
       case 'DECREMENT':
         return { count: state.count - 1 };
       default:
         return state;
     }
   }
   
   function Counter() {
     const [state, dispatch] = useReducer(reducer, { count: 0 });
   
     return (
       <div>
         <p>Count: {state.count}</p>
         <button onClick={() => dispatch({ type: 'INCREMENT' })}>Increment</button>
         <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrement</button>
       </div>
     );
   }
   ```

11. **useMemo和useCallback**： `useMemo` 和 `useCallback` Hooks 用于性能优化。`useMemo` 缓存计算结果，而 `useCallback` 缓存回调函数，以防止在每次渲染时重新创建它们。

    ```jsx
    import React, { useMemo, useCallback } from 'react';
    
    function MyComponent({ data }) {
      const processedData = useMemo(() => processData(data), [data]);
    
      const handleClick = useCallback(() => {
        // 处理点击事件
      }, []); // 传递一个空数组以防止重新创建回调
    }
    ```

这只是React中一些常用的Hooks，还有其他Hooks，如`useRef`、`useLayoutEffect`等，它们可以用于更多特定的场景。选择使用哪些Hooks取决于你的组件需求和项目的特定情况。



