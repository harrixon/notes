# Axios and Redux-thunk

### Axios

`Promise` based interface for fetching resources from the internet 

for **both frontend browser and backend nodejs** 

same as `AJAX`, `jQuery Ajax` or `fetch` 

```ts
// install
yarn add axios @types/axios
```

#### Basic Syntax

```ts
// GET
import axios from 'axios';

axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// POST
import axios from 'axios';

axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

/* response is an AxiosResponse<T> object containing below major properties :
response.data : response body returned from the API
response.status : http status code from server
response.headers : headers that the server responded with , all lowercase
```

```ts
// other method
axios.delete	(url[, config])
axios.head		(url[, config])
axios.options	(url[, config])
axios.post		(url[, data[, config]])
axios.put		(url[, data[, config]])
axios.patch		(url[, data[, config]])
```

#### Configuration

`AxiosRequestConfig ` with options eg: `withCredentials`, `HTTP Headers`, `Basic Authorization` 

```ts
// example
// Basic auth
axios.get('/api/notes', {
  auth: {
    username: 'alex',
    password: 'bootcamp123'
  }
});

// Custom Headers
axios.get('/api/notes', {
  headers: {
    'Authorization': 'Bearer api-key-required-by-3rd-party',
    'X-SOME-HEADER': 'custom-stuff'
  }
});

// Honoring Cross-Site Cookie (withCredentials)
axios.get('http://api.someotherwebsites.com/api/weather', {
  withCredentials: true
});
```

<a href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/withCredentials">see MDN XMLHttpRequest.withCredentials</a>

#### Typescript

type of `response.data ` is default `any`

```ts
// this eg shows how to specify type for response
interface User {
  username: string;
  gender: string;
  income: number;
}

axios.get<User[]>('/users')
  .then(function(response) {
    console.log(response.data[0].username); // TypeScript pass
    console.log(response.data.username); // TypeScript error, common mistake of missing index
    console.log(response.data[0].password); // TypeScript error. common mistake of getting non exist property
  })
  .catch(function (error) {
    console.log(error);
  });
// The response above is now AxiosResponse<User[]> and response.data is User[].
```

---

# Redux-thunk

```ts
// install
yarn add redux-thunk @types/redux-thunk

// store.ts
import { createStore, applyMiddleware } from "redux";
import { RootState, rootReducer } from './reducers'
import thunk from 'redux-thunk';

export const store = createStore<RootState>(
  rootReducer,
  applyMiddleware(thunk)
);
```

```ts
// previous action creater ,  no async
export function addLink(title:string, url:string):AddLinkAction {
  return {
    type: ADD_LINK,
    link: { title: title, url: url }
  }
}

export function clearLink():ClearLinkAction {
  return {
    type: CLEAR_LINK
  }
}
/* Redux Thunk middleware allows writing action creators that return a function instead of an action */

// with thunk , add a fx to fetch link from somewhere else
export function loadLinkSuccess(links: Link[]): LoadLinkSuccessAction {
	return {
		type: LOAD_LINK_SUCCESS,
		links: links
	};
}

export function loadLinkFailure(): LoadLinkFailureAction {
	return {
		type: LOAD_LINK_FAILURE
	};
}

export function loadLink() {
  return (
  	dispatch: Dispatch<LoadLinkSuccessAction | LoadLinkFailureAction>
  ) => {
      return axios("localhost:8080/api/links")
          .then(res => {
          // get back data , populate list
              let threads = res.data;
              let links = threads.data.children.map((t: any) => ({
                  title: t.data.title,
                  url: t.data.url
              }));
              dispatch(loadLinkSuccess(links));
      	})
          .catch(err => {
              console.log("Failed", err);
              dispatch(loadLinkFailure());
      	});
  	}
}
```

