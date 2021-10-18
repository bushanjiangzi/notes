## Hook指什么？
  - Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

  - Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数。

  - Hook 不能在 class 组件中使用 —— 这使得你不使用 class 也能使用 React。

## Hook 使用规则
  - 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。

  - 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。

  - 只要 Hook 的调用顺序在多次渲染之间保持一致，React 就能正确地将内部 state 和对应的 Hook 进行关联
  （在循环、条件判断或者子函数中调用顺序会发生改变）。

## Hook API

### useState
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

### useEffect
  - 在 React 组件中执行数据获取、订阅或者手动修改过 DOM。我们统一把这些操作称为“副作用”，或者简称为“作用”。

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

### useContext
  1. 创建ThemeContext： `const ThemeContext = React.createContext(themes.light)`

  2. 父组件传值：`ThemeContext.Provider`

  3. 子孙组件接收父组件传值：`const theme = useContext(ThemeContext)`

  ```
  const themes = {
    light: {
      foreground: "#000000",
      background: "#eeeeee"
    },
    dark: {
      foreground: "#ffffff",
      background: "#222222"
    }
  };

  const ThemeContext = React.createContext(themes.light);

  function App() {
    return (
      <ThemeContext.Provider value={themes.dark}>
        <Toolbar />
      </ThemeContext.Provider>
    );
  }

  function Toolbar(props) {
    return (
      <div>
        <ThemedButton />
      </div>
    );
  }

  function ThemedButton() {
    const theme = useContext(ThemeContext);
    return (
      <button style={{ background: theme.background, color: theme.foreground }}>
        I am styled by theme context!
      </button>
    );
  }
  ```


### useReducer
  - 在某些场景下，useReducer 会比 useState 更适用，例如 state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等。
  并且，使用 useReducer 还能给那些会触发深更新的组件做性能优化，因为你可以向子组件传递 dispatch 而不是回调函数 。

  ```
  const initialState = {count: 0};

  function reducer(state, action) {
    switch (action.type) {
      case 'increment':
        return {count: state.count + 1};
      case 'decrement':
        return {count: state.count - 1};
      default:
        throw new Error();
    }
  }

  function Counter() {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
      <>
        Count: {state.count}
        <button onClick={() => dispatch({type: 'decrement'})}>-</button>
        <button onClick={() => dispatch({type: 'increment'})}>+</button>
      </>
    );
  }
  ```

### useCallback
  - 把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。

  - `useCallback(fn, deps) 相当于 useMemo(() => fn, deps)`
  ```
  const memoizedCallback = useCallback(
    () => {
      doSomething(a, b);
    },
    [a, b],
  );
  ```

### useMemo
  - 把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。

  - `const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])`;

### useRef
  - useRef 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内保持不变。

  ```
  function TextInputWithFocusButton() {
    const inputEl = useRef(null);
    const onButtonClick = () => {
      // `current` 指向已挂载到 DOM 上的文本输入元素
      inputEl.current.focus();
    };
    return (
      <>
        <input ref={inputEl} type="text" />
        <button onClick={onButtonClick}>Focus the input</button>
      </>
    );
  }
  ```

### useImperativeHandle
   useImperativeHandle 可以让你在使用 ref 时自定义暴露给父组件的实例值。在大多数情况下，应当避免使用 ref 这样的命令式代码。
   useImperativeHandle 应当与 forwardRef 一起使用。

### useLayoutEffect
  - 其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，
  useLayoutEffect 内部的更新计划将被同步刷新。尽可能使用标准的 useEffect 以避免阻塞视觉更新。

### useDebugValue
  - useDebugValue 可用于在 React 开发者工具中显示自定义 hook 的标签。
