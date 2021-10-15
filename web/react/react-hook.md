## Hook指什么？
  - Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

  - Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数。

  - Hook 不能在 class 组件中使用 —— 这使得你不使用 class 也能使用 React。

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
  和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API，使用 useEffect 调度的 effect 不会阻塞浏览器更新屏幕。同时使得相关的
  代码关联更加紧密

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

  - 值发生变化时才重新生成effect，提高性能，如果数组中有多个元素，即使只有一个元素发生变化，React 也会执行 effect。
  如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（[]）作为第二个参数。这就告诉 React 你的 effect 
  不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。
  ```
    useEffect(() => {
      document.title = `You clicked ${count} times`;
    }, [count]); // 仅在 count 更改时更新
  ```

## Hook 使用规则
  - 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。

  - 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。

  - 只要 Hook 的调用顺序在多次渲染之间保持一致，React 就能正确地将内部 state 和对应的 Hook 进行关联
  （在循环、条件判断或者子函数中调用顺序会发生改变）。
