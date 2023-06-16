---
title: "Overview of my Front-end Development Journey using React Library"
datePublished: Tue Sep 07 2021 17:03:09 GMT+0000 (Coordinated Universal Time)
cuid: cktabn6x504m76gs16s0yh11a
slug: overview-of-my-front-end-development-journey-using-react-library
canonical: https://sharon-jebitok.medium.com/overview-of-my-front-end-development-journey-using-react-library-14cf1d54edeb?source=friends_link&sk=1e92e8b74b30a90bf2e2fc5842280c7e
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1631033691577/dw2heZkUm.png
tags: javascript, reactjs, beginners, frontend-development

---

Over the past year, I have been learning software development through project-based learning. Apart from going through a computer science degree or IT-related course in college or university, there’s an option of going through Bootcamp training or self-taught learning to learn software development. Self-Taught learning is where you put together resources that you’ll use to learn after determining the tech stack you want to learn and also creating a culture of self-discipline and consistency since there’s no one planning for you. For me, I opted for the path of Bootcamp training since I was a complete newbie and I felt I needed to achieve some skills within a given period since I had forgone working and opted to learn full-time. 

More often there are these conversations people in the tech space have of “I learned Javascript then I took a break when I picked up from where I left I can’t get it any longer or I’m learning Javascript and get understand anything…” Yes, learning software development can be overwhelming and at times you have to pay someone to give a proper guide and help to learn in a proper way.

My recent learning experience has been a remote pair programming school for software developers school called Microverse. This has given me an opportunity to collaborate on projects with coding partners and attend daily standup meetings with teammates from different countries and time zones this has boosted my communication skills, collaboration skills, understand different cultures, and work under timelines and on specific design guidelines/user stories. 

For one year of learning the major programming language, I have learned frontend development in Javascript using React library. React is an open-source project created by the developer team at Facebook for creating user interfaces for single-page web applications. React is a view layer MVC app that uses to create custom and reusable components. It's quick and efficient for building UIs.

You can install React Developer Tools extension on your browser this makes your experience working with react easier. It enables you to see components as written.

React boilerplate has a static HTML file with head(react CDN Links of React: react top-level API, ReactDOM: for adding DOM specific methods, and babel: Javascript compiler for ES6 modules), div with an id of root for rendering elements i.e entry point for our application, and script tag where custom code lives.
```
// To generate react boilerplate for starting project

$ npx create-react-app new_app
$ cd new_app
$ npm start
```
Create-react-app configured with everything you need to get started building a react app i.e Live development serve, Webpack compiles react, ES6, auto prefix CSS files, ESLint to lint code mistakes and make sure code adheres to some Javascript module standards. Here is the basic code structure:-
```
| public- index.html/among other static files
| src - react code(components/containers/index.js/state management/css/tests)
|.gitignore
|node_modules
|package.json
```
### Rendering Elements

As we earlier said the HTML file on the public folder has a div with id or root for rendering the elements. The index.js file renders the elements into the reactDOM bypassing App with our app’s root DOM reactDOM will manage the app. The basic structure of the index.js file is:-
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```
ReactDOM is a representation of the UI of our app. Any changes to the state of the application mean the UI of the app needs to be updated. Frequent manipulation of the DOM slows the performance of the App. Changes and updates to reactDOM are fast since its represented by a tree data structure. The virtualDOM gets updates instead of the realDOM making it fast and efficient.

### Components

React is made up of components i.e smaller components that feed into the main component App components but you can customize your own way then the main component is exported into the index.js file. The component files imports react, other components(if needed), npm packages installed(if needed), and stylesheet(CSS file). It also uses the JSX to write react code which will be compiled into es6. 

React uses JSX which is HTML-like syntax that is a combination of Javascript and XML. The JSX tags are self-closing and must end. Javascript expressions embedded i.e variables, functions, and properties on JSX are enclosed on curly braces. JSX is easier to write and understand than creating and appending many elements in Vanilla Javascript(createElement).
```
/* when image is defined elsewhere you can enclose image with curly braces */

const App = (
  <div>
    <h1>React is cool!</h1>
    <h2>Start writing code today!</h2>
    <img src={image} alt={name} />
  </div>
);
```
In react, there are class components and react hooks. The class components use props that are hard-coded for props for properties and those for handling state. React hooks offers the simplicity of code and increases productivity. React hooks are not called inside loops, conditions, or nested functions. There are react hooks like:-

- *useState hook*: which adds some local state i.e returns current state and updated state.

- *useEffect hook*: it performs some side effects i.e used for API calls/fetches, search buttons, cleaning up after running effect. Replace componentDidMount/componentDidUnmount/componentDidUpdate

- *useContext hook*: renders re-render on current context

- *useReducer hook*: manages the local state of complex components with a reducer which passes actions and state(at times there’s the debate between react hooks vs redux for state management)

- *useCallbacks hook*: remembers actual function

- *useCustom hook*: remembers returned value from the function

- **Custom Hooks**:- react offers a convenient feature to use and call other hooks in order to extract component logic into reusable functions. 

### Styling

For react you can style your app using any styling method/framework i.e Bootstrap, styled-components, sass, TailWind CSS, CSS Modules you like/comfortable with, or as per project requirements for uniformity.

### Testing

Testing in react is divided into two i.e Rendering component trees in a simplified test environment and asserting on their output and end-to-end test in a realistic browser environment.

Recommended test tools for testing react components, containers or redux(reducers/actions) are:-

- [Jest](jest.io): It is good for testing React components. It provides a great iteration speed combined with powerful features like mocking modules and timers so you can have more control over how the code executes.

- [React-Testing Library](https://testing-library.com/docs/react-testing-library/intro/): It offers a set of helpers like the [ByTestId](https://testing-library.com/docs/queries/bytestid/) function that let you test React components without relying on their implementation details. This approach makes refactoring a breeze and also nudges you towards best practices for accessibility.

This is an overview of front-end development using React library. You can read more on [React Docs](https://reactjs.org/). Thank you for reading through my article. Can comment to share your views.