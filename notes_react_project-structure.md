# Structure

```ts
// ref folder structure
.
├── node_modules
├── public
    ├── favicon.ico
    ├── index.html
    ├── manifest.json
├── src
    ├── actions
        ├── blog.ts
        ├── link.ts
        ├── login.ts
    ├── reducers
        ├── blog.ts
        ├── link.ts
        ├── user.ts
    ├── components
        ├── BlogList.ts
        ├── LoginPage.ts
        ├── LinkDetail.ts
        ├── LinkList.ts
    ├── model.ts
    ├── store.ts
    ├── App.css
    ├── App.tsx
    ├── App.test.tsx
    ├── index.css
    ├── index.tsx
    ├── logo.svg
    ├── registerServiceWorker.ts
├── .gitignore
├── package.json
├── tsconfig.json
├── tsconfig.test.json
├── tslint.json
├── README.md
```

## Actions and Action Creators

```ts
// actions/link.ts
// === this part same as before ===
import { Action } from "redux";

export const ADD_LINK = 'ADD_LINK';
export type ADD_LINK = typeof ADD_LINK;

export interface AddLinkAction extends Action {
  type: ADD_LINK;
  link: { title: string, url: string };
}

export const CLEAR_LINK = 'CLEAR_LINK';
export type CLEAR_LINK = typeof CLEAR_LINK;

export interface ClearLinkAction extends Action {
  type: CLEAR_LINK;
}

export type LinkActions = AddLinkAction | ClearLinkAction;
// =================================

// === new , define the fx in dispatch (ie: mapDispatchToProps) ===
export function addLink(title:string, url:string):AddLinkAction {
  return {
    type: ADD_LINK,
    link: {
      title: title,
      url: url
    }
  }
}

export function clearLink():ClearLinkAction {
  return {
    type: CLEAR_LINK
  }
}
```

```ts
// with the new struture above now re-write <mapDispatchToProps> in 
// App.tsx
import { addLink, clearLink } from "./actions/link";
const mapDispatchToProps = (dispatch: Dispatch<LinkActions>) => {
    return {
        addLink: () => dispatch(addLink("Accelerate", "http://www.acceleratedhk.com")),
        clearLink: () => dispatch(clearLink()),
    }
}
```

---

## Reducers and combineReducers

```ts
// combineReducer : an index for reducers
// reducers/index.ts
import { combineReducers } from 'redux'
import { blogReducer, BlogState } from './blog'
import { linkReducer, LinkState } from './link'
import { userReducer, UserState } from './user'

export interface RootState {
  blogs: BlogState,
  links: LinkState,
  users: UserState
}

export const rootReducer = combineReducers<RootState>({
  links: linkReducer
})
```

```ts
// reducers/link.ts
import { LinkActions, ADD_LINK, CLEAR_LINK } from '../actions/link'

export type LinkState = {
  title: string,
  url: string
}[]

// this part copied from old rootReducer
export const linkReducer = (state: LinkState = [], action: LinkActions) : LinkState => {
  // Use switch to handle different actions
  switch (action.type) {
    case ADD_LINK:
      return state.concat([action.link]); // Use concat to add a new link
    case CLEAR_LINK:
      return []; // Reset the link
    default:
      return state; // Do not change the state in case of any other actions
  }
};
```

```ts
// rootReducer final output
{
  "links": [
    // links here
  ],
  "users": [
    // users here
  ],
  "blogs": [
    // blogs here
  ]
}
```

```ts
// store.ts
import { createStore } from "redux";
import { RootState, rootReducer } from './reducers'

export const store = createStore<RootState>(rootReducer);
```

