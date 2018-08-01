# Action, Reducer and Dispatch

-  all states are immutable 

-  whenever an `Action` is dispatched to the store : 

  Redux store will call the root `Reducer` for a new state

```ts
onEdit() -- dispatch(data + action) --> root reducer
									--> reducer --> setState @ store
					re-populate dom <-- mapStateToProps <--
```

---

#### Action

```ts
// action = plain object with a key type of string , plus additonal info
{
  "type": "ADD_LINK",
  "link": {
    "title": "Bing",
    "url": "http://www.bing.com"
  }
}
```

```ts
// to define Action is ts (use ts to prevent typo & bugs)
import { createStore, Dispatch, Action } from "redux";

// Define ADD_LINK const and its type
const ADD_LINK = 'ADD_LINK';		// a simple const to avoid typo
type ADD_LINK = typeof ADD_LINK;	/* ts syntax, to set <ADD_LINK> as one of variable accepted 									in someAction.type */

// Define how AddLinkAction should look like
interface AddLinkAction extends Action {	// Action: basic type of redux action
  type: ADD_LINK;
  link: {
    title: string,
    url: string
  };
}

// Define CLEAR_LINK const and its type
const CLEAR_LINK = 'CLEAR_LINK';
type CLEAR_LINK = typeof CLEAR_LINK;

// Define ClearLinkAction. No additional information is needed
interface ClearLinkAction extends Action {
  type: CLEAR_LINK;
}

// Collection of action types for easier integration
type LinkActions = AddLinkAction | ClearLinkAction;
```

---

#### Upgrading reducer

```ts
// last time :
const rootReducer = (state: RootState) => {
  return { links: [
              { title: 'Google', url: 'http://www.google.com' },
              { title: 'Yahoo', url: 'http://www.yahoo.com' },   ]}
};

// new : 
const rootReducer = (state: RootState, action: LinkActions /* optional param here */) => {
  // Use switch to handle different actions		  ^
  switch (action.type) {	// -------------------|
    case ADD_LINK:
      return {
        links: state.links.concat([action.link]) // Use concat to add a new link
      }
    case CLEAR_LINK:
      return {
        links: [] // Reset the link
      }
    default:
      return state; // Do not change the state in case of any other actions
  }
};
/* use <state.links.concat> (which create a new array) instead of <state.links.push>
because the original state should never be modified or mutated
a new array shall be returned */
```

---

#### Dispatch

```ts
// add 2 new btn
// use connect() to link up new btn and dispatch
import { Dispatch } from "redux"; // We have to import `Dispatch` type

interface LinkListProps {
  links: { title: string, url: string }[],
  addLink: () => void,		// Add a new mapped prop
  clearLink: () => void		// Add a new mapped prop
}

const PureLinkList = (props: LinkListProps) => {
  return (
    <div>
      <button onClick={props.addLink}>New Link</button>
      <button onClick={props.clearLink}>Clear</button>
      {props.links.map(l => (
        <div>{l.title} - {l.url}</div>
      ))}
    </div>
  );
}

const mapStateToProps = (state: RootState) => { return { links: state.links } }

// Add the `mapDispatchToProp`
const mapDispatchToProps = (dispatch: Dispatch<LinkActions>) => {
  return {
    addLink: () => dispatch({
      type: ADD_LINK,
      link: {
        title: 'Accelerate',
        url: 'http://www.acceleratedhk.com'
      }
    }),
    clearLink: () => dispatch({
      type: CLEAR_LINK
    })
  }
}

// Link with connect()
const LinkList = connect(mapStateToProps, mapDispatchToProps)(PureLinkList)
```

---

### Combined

```ts
import * as React from "react";
// import { render } from "react-dom";
import { connect, Provider } from "react-redux";
import { Action, createStore, Dispatch } from "redux";

// Define ADD_LINK const and its type
const ADD_LINK = 'ADD_LINK';
type ADD_LINK = typeof ADD_LINK;

// Define how AddLinkAction should look like
interface AddLinkAction extends Action {
    type: ADD_LINK;
    link: {
        title: string,
        url: string
    };
}

// Define CLEAR_LINK const and its type
const CLEAR_LINK = 'CLEAR_LINK';
type CLEAR_LINK = typeof CLEAR_LINK;

// Define ClearLinkAction. No additional information is needed
interface ClearLinkAction extends Action {
    type: CLEAR_LINK;
}

// Collection of both for easier integration
type LinkActions = AddLinkAction | ClearLinkAction;

// tslint:disable-next-line:interface-name
interface RootState {
    // tslint:disable-next-line:array-type
    links: {
        title: string,
        url: string
    }[],
}

const rootReducer = (state: RootState, action: LinkActions /* add parameter here */) => {
    if (typeof (state) === "undefined") {
        return { links: [] }
    }
    else {
        // Use switch to handle different actions
        switch (action.type) {
            case ADD_LINK:
                return {
                    links: state.links.concat([action.link]) // Use concat to add a new link
                }
            case CLEAR_LINK:
                return {
                    links: [] // Reset the link
                }
            default:
                return state; // Do not change the state in case of any other actions
        }
    }
};

const store = createStore<RootState, Action, {}, {}>(rootReducer);

interface LinkListProps {
    links: Array<{
        title: string,
        url: string
    }>,
    addLink: () => void, // Add a new mapped prop
    clearLink: () => void, // Add a new mapped prop
}

const PureLinkList = (props: LinkListProps) => {
    return (
        <div>
            <button onClick={props.addLink}>New Link</button>
            <button onClick={props.clearLink}>Clear</button>
            {props.links.map(l => (
                <div>{l.title} - {l.url}</div>
            ))}
        </div>
    );
}

const mapStateToProps = (state: RootState) => {
    if (typeof (state) === "undefined") {
        return { links: [] }
    }
    else {
        return { links: state.links }
    }
}

// Add the `mapDispatchToProp`
const mapDispatchToProps = (dispatch: Dispatch<LinkActions>) => {
    return {
        addLink: () => dispatch({red
            type: ADD_LINK,
            link: {
                title: 'Accelerate',
                url: 'http://www.acceleratedhk.com'
            }
        }),
        clearLink: () => dispatch({
            type: CLEAR_LINK
        })
    }
}

// Link with connect()
const LinkList = connect(mapStateToProps, mapDispatchToProps)(PureLinkList)

const App = () => (
    <Provider store={store}>
        <div>
            <h2>Links</h2>
            <LinkList />
            {/* <h2>Users</h2>
            <UserList /> */}
        </div>
    </Provider>
);

export default App;
```

