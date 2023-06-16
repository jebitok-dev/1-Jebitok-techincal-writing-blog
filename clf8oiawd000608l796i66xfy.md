---
title: "Removing Vite from a React.js Project"
seoTitle: "How to Remove Vite from a React.js Project"
datePublished: Tue Mar 14 2023 20:01:15 GMT+0000 (Coordinated Universal Time)
cuid: clf8oiawd000608l796i66xfy
slug: removing-vite-from-a-reactjs-project
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1678823996524/77b4f18b-f048-4c56-b855-2a6a1cacc32d.png
tags: reactjs, vite

---

React.js is a popular Javascript framework used for the front-end development for web applications. Most software developers often make comments on how creating a new react.js application takes a long time if you use the conventional provided [React.js documentation](https://beta.reactjs.org/learn/start-a-new-react-project).

```javascript
npx create-react-app my-app
cd my-app
npm start
```

With the growth of the state of web development over the years there are tools like [Vite](https://vitejs.dev/) and [Parcel](https://parceljs.org/getting-started/webapp/) that have been created to support development while using modern frameworks and even programming languages.

## Vite

In this case, we are talking about [Vite](https://vitejs.dev/), which is said to be the "Next Generation Frontend Tooling" that enables you to get ready for a development environment that can finally catch up with you. Vite offers these unique features:

* Instant server start
    
* Lighting fast HMR
    
* Rich features i.e it supports Typescript, JSX, and CSS among others
    
* Optimized build
    
* Universal plugins
    
* Fully typed APIs
    

## How to Install Vite

```javascript
// With NPM
npm create vite@latest
// With Yarn
yarn create vite
```

### How to Set Up React.js project with Vite

```javascript
$ yarn create vite
```

When you run the command you see output like this on your terminal and it requires you to select a vite-project name

```javascript
// output
yarn create v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-vite@4.1.0" with binaries:
      - create-vite
      - cva
? Project name: › vite-project
```

you can name it `react-vite-1` just type it and it will replace highlighted vite-project

```javascript
react-vite-1
```

when you enter the project name you'll get an output that requires you to select a framework you want to work with. You can scroll with down and up arrows to select the one you want to work then submit. For this case, we'll select `React`

```javascript
// output
✔ Project name: … react-vite-1
? Select a framework: › - Use arrow-keys. Return to submit.
❯   Vanilla
    Vue
    React
    Preact
    Lit
    Svelte
    Others
```

After selecting a framework we have to select a variant that is either `Javascript` or `Typescript` template

```javascript
// output
✔ Project name: … react-vite-1
✔ Select a framework: › React
? Select a variant: › - Use arrow-keys. Return to submit.
❯   JavaScript
    TypeScript
    JavaScript + SWC
    TypeScript + SWC
```

we can select the `typescript` one and our project will be scaffolded successfully we can run the following commands

```javascript
cd react-vite-1
yarn
yarn dev
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678824168804/db0b7122-efaf-487e-a5e0-59d9b87673c6.png align="center")

With this, you can go ahead and build your react.js project of choice that uses `typescript` so you have to adhere to the different types also uses `Vite` to ease the development process like starting live on localhost, installing packages, and even bundling.

### Removing Vite from React.js Project

At times you're working on a project and you initialized using Vite but you get to some point while building and get some error like Vite not being able to bundle an import of images within a `useEffect` hook and you need to remove Vite since you can't debug the issue within the short time you have despite the fact

Follow these steps to remove Vite:

1. Remove the Vite dependency from your project by running the following command :
    
    ```javascript
    npm uninstall vite
    ```
    
2. Remove the vite configuration file named `vite.config.js` from your project root folder
    
3. Update your package.json file to remove any vite-specific scripts or configurations:
    
    ```javascript
    "scripts": {
      - "start": "vite",
      + "start": "react-scripts start",
      "build": "react-scripts build",
      "test": "react-scripts test",
      ...
    }
    ```
    
4. Update any imports that may have been using Vite-specific aliases or paths e.g use of `@` to import files from the `src` folder with vite. This might bring a lot of issues but take your time to ensure all imports are well imported
    
5. Add `index.html` file public folder with a `div` that has a `root` id
    
6. Run npm install to ensure all dependencies are up to date and any unused packages are removed. At this point, Vite is removed from your project and you can continue building your project.
    
7. Ensure when you start the server it can detect some browsers by adding the defaults to your `package.json` file in your React app. Run the following commands in your terminal
    
    ```javascript
    npm install postcss autoprefixer
    npx browserslist // generates list of targeted browsers 
    ```
    
    Go back to your package.json file and add browserlist section at the top next to the scripts like this
    
    ```javascript
    {
      "name": "my-react-app",
      "version": "0.1.0",
      "private": true,
      "dependencies": {
        // your dependencies
      },
      "browserslist": [
        "last 1 version",
        "> 1%",
        "not dead"
      ]
    }
    ```
    
    Thank you for reading through my article, you can leave a comment or we connect more on [Twitter](https://twitter.com/SharonJebitok) or [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/). I've been a bit inconsistent in terms of writing but hoping to write more soon. Have a good time and hoping to read yours too.