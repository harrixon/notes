# React-Redux

#### installation

```cmd
// to create react envir.
npm install -g create-react-app

// to transpile .ts to .js
npm install -g ts-node typescript @types/node
```

#### new project

~~~cmd
// create new project
create-react-app my-app --scripts-version=react-scripts-ts

// yarn server @localhost:3000
yarn start

// other cmd
test
run build
run eject
~~~

#### Plugins

```cmd
// react-router
yarn add react-router-dom @types/react-router-dom

// reactstrap
yarn add reactstrap @types/reactstrap

// redux & react-redux
yarn add redux react-redux @types/react-redux

// redux-logger
yarn add redux-logger @types/redux-logger

// axios
yarn add axios @types/axios

// redux-thunk
yarn add redux-thunk @types/redux-thunk

// FB login
yarn add react-facebook-login @types/react-facebook-login

// test case: jest & enzyme
yarn add jest-enzyme enzyme @types/enzyme enzyme-adapter-react-16 @types/enzyme-adapter-react-16
```

#### Backend

```cmd
// passport
npm install passport passport-jwt jwt-simple --save
```

---

## Redux Store

A Single Store Approach , use `<Provider>` and `connect()` to link up any individual component to Redux Store , state in Redux Store is **immutable** and has **no auto-binding** 

`yarn add  redux react-redux @types/react-redux`

## Create a Store

```ts
import * as React from "react";
import { connect, Provider } from "react-redux";
import { Action, createStore } from "redux";

// everything state in the app is stored in rootstate
// and we define the interface(structure) how info should be stored
interface IRootState {
    // v this is an array of links this app is storing
  links: Array<{
    title: string,
    url: string
  }>,
  users: Array<{
    username: string
  }>
}

const rootReducer = (state: IRootState) => {
  return {
    links: [
      { title: 'Google', url: 'http://www.google.com' },
      { title: 'Yahoo', url: 'http://www.yahoo.com' },
    ],
      // EX A
    users: [
      { username: "user1" },
      { username: "user2" },
    ]
      // ===
  }
};

// createStore<T>() is a fx provided by Redux to create a root store
// with the interface of state defined in the type generic argument <IRootState>
const store = createStore<IRootState, Action, {}, {}>(rootReducer);

// consider this component (.tsx)
// using in React: <PureLinkList links={this.state.links} />
interface LinkListProps {
  links: {
    title: string,
    url: string
  }[]
}

const PureLinkList = (props: LinkListProps) => {
  return (
    <div>
      {props.links.map(l => (
        <div>{l.title} - {l.url}</div>
      ))}
    </div>
  );
}

// to use above compo' in redux, we use connet() in react-redux
import { IRootState } from '../redux/store';
const mapStateToProps = (state: IRootState) => {
  return {
    links: state.links
  }
}

const LinkList = connect(mapStateToProps)(PureLinkList)

// EX A
interface IUserList {
  users: Array<{
    username: string
  }>
}

const PureUserList = (props: IUserList) => {
  return (
    <div>
      {props.users.map(u => {
        return (<div key={u.username}>{u.username}</div>)
      })}
    </div>
  )
}

const mapStateToProps2 = (state: RootState) => {
  return {
    users: state.users
  }
}
// ====

// using in app.tsx
// also need to use <Provider> in the root of react application, usually <App>
// <Provider> provides its children access to store through connect()
// {store} attribute = redux store created above
const App = () => (
  <Provider store={store}>
    <div>
      <h2>Links</h2>
      <LinkList />
      // EX A
      <h2>Users</h2>
      <UserList />
    // ===
    </div>
  </Provider>
);

render(<App />, document.getElementById("root"));
```

#### how to use connect()()

```ts
interface IUserList { users: Array<{ username: string }> }

const PureUserList = (props: IUserList) => {
  return (
    <div>
      {props.users.map(u => { return (<div key={u.username}>{u.username}</div>) })}
    </div>
  )
}

const mapStateToProps2 = (state: RootState) => {
  return { users: state.users }
}

// UserList = array of virtual dom outputs
// PureUserList is a fx that define and create virtual doms
// mapStateToProps2  fetch wtever that is needed in PureUserList
const UserList = connect(mapStateToProps2)(PureUserList)
// outPut[] = connect(structure fx)(mapping)
```

