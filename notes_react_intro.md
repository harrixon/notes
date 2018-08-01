# Intro To React

### Installation

```cmd
// first time on machine
npm install -g create-react-app

// in ur project folder
create-react-app my-app --scripts-version=react-scripts-ts

// after installing , start the server
cd my-app/
npm start
```

### Essentials libraries 

3 essential libraries

- React : provides the entry point to react `Component` and its `prop`, `state` and `children` 
- React-dom :  provides linkage between `react` and `DOM` so that the `virtual DOM` in `react` 
- React-script : provides shorthand for beginners to start their `React` project,also a bundler and a mini web server with livereload functionality. 

### Folder Structure

`public` : static assets for `public/index.html` . Only files inside this folder can be accessed from `index.html` 

`public/index.html` is the entry point of your react website , **This file cannot be renamed or deleted** 

`src` All the source files located here are combined into our React application 
	(`js`, `jsx`, `tsx` ,`css`,`jpg`, `png`, `svg` )

`src/index.tsx`: The entry point of your application  , **This file cannot be renamed or deleted** 

### Common Cmd

`npm start`

`npm test`

`npm run build`

`npm run eject` : **one-way operation** ,**you can’t go back!** 





---

`index` > `app` > `switch , route` > `render components`

`@component` has

​	`props`  given by parent element , including data and callback (pass data up hierarchy to change parent's state)

​	` state` of its own (mostly data)

















