### react 框架的特点

- 声明式开发 - 减少大量 dom 操作
- 可以与其他框架并存
- 组件化
- 单向数据流 - 父组件往子组件传数据，子组件不能改变父组件传递下来的数据
- 视图层框架 - 在大型项目中还需引入 flux，redux 等数据层框架
- 函数式编程 - 易于前端自动化测试

### 生命周期函数

- Initialization
- Mounting
  - componentWillMount - 挂载前
  - render
  - componentDidMount - 挂载完成
- Updation
- UnMounting

### 动画

- `react-transition-group` - 动画库

### redux 中间件

- 原始的`redux`

  1. 创建 store 仓库

  ```
    import { createStore } from 'redux'
    import reducer from './reducer'

    const store = createStore(reducer)

    export default store
  ```

  2. 创建 reducer 管理 store 中心

  ```
    import {
      CHANGE_INPUT_VALUE,
      ADD_TODO_ITEM,
      DELETE_TODO_ITEM,
    } from './actionTypes'

    const defaultState = {
      inputValue: "",
      list: ["学习vue", "学习react"],
    };

    // reducer可以接受state，但是不能直接改变state，需要进行深拷贝
    const reducer = (state = defaultState, action) => {
      // console.log(action)
      if (action.type === CHANGE_INPUT_VALUE) {
        const newState = JSON.parse(JSON.stringify(state))
        newState.inputValue = action.value
        return newState
      }
      if (action.type === ADD_TODO_ITEM) {
        const newState = JSON.parse(JSON.stringify(state))
        if (newState.inputValue) {
          newState.list.push(newState.inputValue)
        }
        newState.inputValue = ''
        return newState
      }
      if (action.type === DELETE_TODO_ITEM) {
        const newState = JSON.parse(JSON.stringify(state))
        newState.list.splice(action.index, 1)
        return newState
      }
      return state;
    };

    export default reducer
  ```

  3. 创建提交修改 store 的 action

  ```
    import axios from 'axios'
    import {
      CHANGE_INPUT_VALUE,
      ADD_TODO_ITEM,
      DELETE_TODO_ITEM,
      INIT_LIST_ACTION,
    } from './actionTypes'

    const getChangeInputValue = (value) => ({
      type: CHANGE_INPUT_VALUE,
      value
    })
    const getAddTodoItem = (value) => ({
      type: ADD_TODO_ITEM,
      value
    })
    const getDeleteTodoItem = (value) => ({
      type: DELETE_TODO_ITEM,
      value
    })
    const initListAction = (value) => ({
      type: INIT_LIST_ACTION,
      value
    })

    // redux-thunk
    const getTodoList = () => {
      return (dispatch) => {
        axios.get('../../json/todoList.json')
        .then((res) => {
          console.log(res)
          const action = initListAction(res.data.list)
          dispatch(action)
        })
        .catch((err) => {
          console.log(err)
        })
      }
    }

    export {
      getChangeInputValue,
      getAddTodoItem,
      getDeleteTodoItem,
      initListAction,
      getTodoList
    }
  ```

  4. 获取 store 中的 state 数据及修改

  ```
    import React, { Component } from "react";
    import "../assets/css/main.css";
    import store from '../store'
    import { getChangeInputValue, getAddTodoItem, getDeleteTodoItem, getTodoList } from '../store/creatActions'
    import TodoListUI from './TodoListUI'

    class TodoList extends Component {
      constructor(props) {
        super(props);
        // 初次获取store数据
        this.state = store.getState()
        // `store.subscribe`j监听store数据变化手动更新重新获取store数据
        store.subscribe(this.handleStoreChange.bind(this))
      }
      render() {
        return ();
      }
      // 挂载完成
      componentDidMount() {
        /**
        * redux-thunk(异步请求返回数据修改state)
        */
        const action = getTodoList()
        store.dispatch(action)
      handleInputChange(e) {
        const action = getChangeInputValue(e.target.value)
        store.dispatch(action)
      }
    }

    export default TodoList;
  ```

- `redux-saga`

- `redux-thunk`的用法

  1. 在`/src/App`文件引入 store`import store from './store';`

  ```
    // `/src/App.js`
    import React, { Component } from 'react';
    import { Provider } from 'react-redux';
    import { BrowserRouter, Route } from 'react-router-dom';
    import Login from './pages/login';
    import store from './store';

    class App extends Component {
      render() {
        return (
          <Provider store={store}>
            <BrowserRouter>
              <div>
                <Route path='/login' exact component={Login}></Route>
              </div>
            </BrowserRouter>
          </Provider>
        );
      }
    }

    export default App;
  ```

  2. 在`/src/store/index.js`中引入`redux-thunk`

  ```
    // `/src/store/index.js`
    import { createStore, compose, applyMiddleware } from 'redux';
    import thunk from 'redux-thunk';
    import reducer from './reducer';

    const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
    const store = createStore(reducer, composeEnhancers(
      applyMiddleware(thunk)
    ));

    export default store;
  ```

  3. 在`/src/store/reducer.js`中引入`redux-immutable`，`combineReducers`方法将多个组件中的 state 数据转合并起来

  ```
    // `/src/store/reducer.js`
    import { combineReducers } from 'redux-immutable';
    import { reducer as loginReducer } from '../pages/login/store';
    const reducer = combineReducers({
      login: loginReducer
    });

    export default reducer;
  ```

  4. 在`/src/login/store/reducer.js`中引入`immutable`，`fromJS`方法将 state 数据转为`immutable`，这将不允许 state 的数据被直接获取和修改

  ```
    // `/src/pages/login/store/index.js`
    import reducer from './reducer';
    import * as actionCreators from './actionCreators';
    import * as constants from './constants';

    export { reducer, actionCreators, constants };

    // `/src/pages/login/store/reducer.js`
    import { fromJS } from 'immutable';
    import * as constants from './constants';

    const defaultState = fromJS({
      login: false
    });

    const defaultReducer = (state = defaultState, action) => {
      switch(action.type) {
        case constants.CHANGE_LOGIN:
          return state.set('login', action.value);
        case constants.LOGOUT:
          return state.set('login', action.value);
        default:
          return state;
      }
    }
    export default defaultReducer

    // `/src/pages/login/store/actionCreators.js`
    import axios from 'axios';
    import * as constants from './constants';

    const changeLogin = () => ({
      type: constants.CHANGE_LOGIN,
      value: true
    })

    export const logout = () => ({
      type: constants.LOGOUT,
      value: false
    })

    export const login = (accout, password) => {
      return (dispatch) => {
        axios.get('/json/login.json?account=' + accout + '&password=' + password).then((res) => {
          const result = res.data.data;
          if (result) {
            dispatch(changeLogin())
          }else {
            alert('登陆失败')
          }
        })
      }
    }

    // `/src/pages/login/store/constants.js`
    export const CHANGE_LOGIN = 'login/CHANGE_LOGIN';
    export const LOGOUT = 'login/LOGOUT';
  ```

  5. 在`/src/login/index.js`中`connect`方法将连接`state`和`dispatch`，通过`state.getIn(['login', 'login'])`方式获取`immutable`数据

  ```
    // `/src/pages/login/index.js`
    import React, { PureComponent } from 'react';
    import { Redirect } from 'react-router-dom';
    import { connect } from 'react-redux';
    import { LoginWrapper, LoginBox, Input, Button } from './style';
    import { actionCreators } from './store';

    class Login extends PureComponent {
      render() {
        const { loginStatus } = this.props;
        if (!loginStatus) {
          return (
            <LoginWrapper>
              <LoginBox>
                <Input placeholder='账号' ref={(input) => {this.account = input}}/>
                <Input placeholder='密码' type='password' ref={(input) => {this.password = input}}/>
                <Button onClick={() => this.props.login(this.account, this.password)}>登陆</Button>
              </LoginBox>
            </LoginWrapper>
          )
        }else {
          return <Redirect to='/'/>
        }
      }
    }

    const mapState = (state) => ({
      loginStatus: state.getIn(['login', 'login'])
    })

    const mapDispatch = (dispatch) => ({
      login(accountElem, passwordElem){
        dispatch(actionCreators.login(accountElem.value, passwordElem.value))
      }
    })

    export default connect(mapState, mapDispatch)(Login);
  ```

### styled 样式

- `/src/pages/login/style.js`

  ```
  import styled from 'styled-components';

  export const LoginWrapper = styled.div`
    z-index: 0;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    top: 56px;
    background: #eee;
  `;

  export const LoginBox = styled.div`
    width: 400px;
    height: 180px;
    margin: 100px auto;
    padding-top: 20px;
    background: #fff;
    box-shadow: 0 0 8px rgba(0,0,0,.1);
  `;

  export const Input = styled.input`
    display: block;
    width: 200px;
    height: 30px;
    line-height: 30px;
    padding: 0 10px;
    margin: 10px auto;
    color: #777;
  `;

  export const Button = styled.div`
    width: 220px;
    height: 30px;
    line-height: 30px;
    color: #fff;
    background: #3194d0;
    border-radius: 15px;
    margin: 10px auto;
    text-align: center;
  `;
  ```

### 路由

- App.js
  ```
  class App extends Component {
    render() {
      return (
        <Provider store={store}>
          <BrowserRouter>
            <div>
              <Header />
              <Route path='/' exact component={Home}></Route>
              <Route path='/login' exact component={Login}></Route>
              <Route path='/write' exact component={Write}></Route>
              <Route path='/detail/:id' exact component={Detail}></Route>
            </div>
          </BrowserRouter>
        </Provider>
      );
    }
  }
  ```
- 路由传参

  1. 通过 params

     > 在标签中传: `<Route path='/detail/:id' exact component={Detail}></Route>`;获取参数`this.props.match.params.id`

     > js: this.props.history.push( '/sort/' + id )

  2. 通过 query

     > `<Link to={{ path : ' /sort ' , query : { name : 'sunny' }}}>`;

     > `this.props.history.push({ path : '/sort' ,query : { name: ' sunny'} })`

     > 取参数：`this.props.location.query.name`
