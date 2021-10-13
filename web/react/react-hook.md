## Hook指什么？
  - Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

  - Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数。

  - Hook 不能在 class 组件中使用 —— 这使得你不使用 class 也能使用 React。

## Hook 使用规则
  - 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。

  - 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。

## useState
  - useState 会返回一对值：当前状态和一个让你更新它的函数，useState 唯一的参数就是初始 state，可以是数字，字符串，对象，布尔值等。
  ```
  import React, { useState } from 'react';

  function Example() {
    // 声明一个叫 “count” 的 state 变量。
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

## Effect Hook
- 在 React 组件中执行数据获取、订阅或者手动修改过 DOM。我们统一把这些操作称为“副作用”，或者简称为“作用”。

  ### useEffect
  - useEffect 就是一个 Effect Hook，给函数组件增加了操作副作用的能力。它跟 class 组件中的 componentDidMount、componentDidUpdate 
  和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API

  ```
    // 相当于 componentDidMount 和 componentDidUpdate:
    useEffect(() => {
      // 使用浏览器的 API 更新页面标题
      document.title = `You clicked ${count} times`;
    });
  ```
  - 副作用函数还可以通过返回一个函数来指定如何“清除”副作用。如：
  ```
    useEffect(() => {
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    });
  ```