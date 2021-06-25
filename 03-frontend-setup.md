# THE **MERN** STACK — FRONT-END SETUP

For the front-end we use React and Chakra UI for styling.

## React and Chakra UI

For setting up react, use [`create-react-app`](https://create-react-app.dev). It is nothing but just a simple npm package developed by Facebook team to simplify the process of creating development environment for react. There are some templates you can generate using `create-react-app`. For this project, we generate a create-react-app template which has chakra UI installed. Use the following command to create react environment with chakra UI.

```
npx create-react-app <name-of-folder> --template @chakra-ui
```

You can name the directory whatever you want. I prefer to name it as fornt-end.

Now follow the instructions appear in the terminal.

```
cd front-end
npm start
```

After running `npm start`, it runs a server on posrt 3000 and open the browser automatically. If you see the following image in the browser, you are good to go. 

![React-app]()

If any error occurs, visit the [create react app](https://create-react-app.dev) website for trouble-shooting.

## Dependencies

### Axios

Axios is a small dependency which makes it easy for us to make promise based HTTP requests.

```
npm install axios
```

### React oruter DOM

React Router DOM is a dependency for making navigation between components of our application.

```
npm install react-router-dom
```

### Timeago.js

It is a small package to format date and time in form `*** time ago`.

```
npm install timeago.js
```

## Git

Additionally, you should download git. It is a version control system that enables tracking code changes, code sharing, and collaboration.

You can download it from the [official website](https://git-scm.com/downloads). Once you complete installing git, check by running the command `git --version` in your command line. It should give you the version you installed. You don't have to master in using git for this project but it is recommended to have good knowledge of git as a developer.
