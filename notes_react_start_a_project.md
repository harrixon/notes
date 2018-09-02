# React.js

## Start a project

### Frontend

> assume yarn is already installed

```cmd
yarn install -g create-react-app

cd project_folder
create-react-app frontend --scripts-version=react-scripts-ts

# the app is hosted on http://localhost:3000/
cd frontend/
yarn start

# useful package
yarn add reactstrap @types/reactstrap
yarn add react-router-dom @types/react-router-dom
yarn add redux react-redux @types/react-redux
yarn add redux-logger @types/redux-logger
yarn add axios @types/axios
yarn add redux-thunk @types/redux-thunk
yarn add react-facebook-login @types/react-facebook-login
yarn add jest-enzyme enzyme @types/enzyme enzyme-adapter-react-16 @types/enzyme-adapter-react-16
```

#### how to install plugin

```cmd
# for package written in typescript
# in .ts file, import { createStore, combineReducers, applyMiddleware, compose } from 'redux';
yarn add redux

# for package written in JavaScript, with a type definition file provided by the community
# in .ts file, import { Provider } from 'react-redux';
yarn add react-redux @types/react-redux

# for package written in JavaScript only
# in .ts file, const theModule = require('some-not-so-famous-module');
# or: const theModule = require('some-not-so-famous-module').default;
yarn add some-not-so-famous-module
```



---

### Backend

> using Express server as API server

```cmd
cd project_folder/backend
yarn init
yarn add -g ts-node typescript @types/node

# useful packages
yarn add body-parser
yarn add cors
yarn add dotenv
yarn add express
yarn add knex
yarn add passport passport-jwt jwt-simple --save
```

#### index.js

```javascript
// in index.js add:
require('ts-node/register');
require('./app'); // or any name of your entry point
```

