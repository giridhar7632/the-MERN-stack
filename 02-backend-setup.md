# THE **MERN** STACK — BACK-END SETUP

Let's start setting up our development environment for creating the Notes Taking App.

You can setup the project locally or cloud editors like [Repl.it](https://replit.com) or [Codesandbox](https://codesandbox.io). I assume you are going to develop locally and guide you through the process.

## Node.js Installation

First of all, you should install Node.js into your system. You can download Node.js application from the [official website](https://nodejs.org/en/download/). I suggest installing the LTS (**L**ong **T**erm **S**upport) version. 14.7.0 is the latest version at the time of writing this tutorial. Make sure you install the latest version for best features.

![Node.js official website](https://cloud-cc5w17pxq-hack-club-bot.vercel.app/3node.js-login.png)

Once you complete installing Node.js on your computer, you can check by typing `node -v` in your terminal. This gives the version of the Node.js you installed. One of the most important tool, the `npm` is also installed along with Node. You can check the version of npm by running the command `npm -v` in your terminal.

Once you have Node installed, create a folder anywhere and you can name it as `Notes-takin-app`. Open the folder in your favourite code editor.

There are many powerful code editors for web development like [Atom](https://atom.io/), [WebStorm](https://www.jetbrains.com/webstorm/) and so on, my favourite is [Visual Studio Code](https://code.visualstudio.com/) which is used by most of the developers. Many of these are code editors has support for modern web development workflow, including support for MERN stack technologies.

Open the folder in your terminal. Most of the code editors has built-in terminal for making the development experience easy. Or you can just open the folder in your machine termial. Type `npm init`. This will initiate a node.js project with a `package.json` file which contains the essential information about your project. It asks some basic questions about your project, you can type the answer or just press `Enter` for everything. You can avoid the questions by passing `npm init -y` command, which defaults all the question to `yes`. 

Your `package.json` file may look like this:

```json
{
  "name": "notes-taking-app",
  "version": "1.0.0",
  "description": "a simple MERN stack notes takin app with basic auth and CRUD functionality",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "node.js",
    "mongodb",
    "express",
    "MERN-stack",
    "full-stack"
  ],
  "license": "ISC"
}

```

This `package.json` file contains all the information about the packages we used in for this project. Learn more about `package.json` file [here](https://docs.npmjs.com/cli/v7/configuring-npm/package-json).

## Express Installation

[Express](https://www.npmjs.com/package/express) is a package that is available on `npm`. Let's install Express using `npm` into our project.

You can install Express by typing the following command in the terminal.

```
npm install express
```

This will install all the necessary dependencies for using Express. In your `package.json` file, you can see a new field called "dependencies". This field will contain the dependencies you used for the project.

```json
...
"dependencies": {
    "express": "^4.17.1"
  }
...
```

This also creates a new file `package-lock.json` which contains very detailed information about the dependencies used in the project. You do not have to concern about this file, it will be automatically updated every time you install a dependency by npm.

A new folder called `node_modules` will also appear. This contains the installed dependencies.

We need a few more dependencies for making our development easier.

```
npm install body-parser cors
```

## MondoDB Installation

For connnecting to MongoDB, we have to install the dependency called `mongoose`. [Mongoose](https://www.npmjs.com/package/mongoose) is used to connect with MongoDB Atlas.

```
npm install mongoose
```

Create a file named `index.js` in the root folder. It is the entry point of our application. Try console logging any thing to see whether everything is working properly.

Last but not least, you have to install a package called `nodemon`. [Nodemon](https://www.npmjs.com/package/nodemon) makes the development of Node.js applications flexible by automatically restarting the server when file changes in the directory are detected. We install it as a development dependency. Learn more about dependencies in npm [here](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file).

```
npm install --save-dev nodemon
```

This command installs `nodemon` and saves as a dev dependency. Now let's define some scripts.

In the `scripts` of `package.json` file, create two scripts.

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
}
```

Now try running `npm run dev` command in your terminal. It runs the `index.js` file with the help of nodemon. Your final `package.json` file will loook something like this:

```json
{
  "name": "notes-taking-app",
  "version": "1.0.0",
  "description": "a simple MERN stack notes takin app with basic auth and CRUD functionality",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "keywords": [
    "node.js",
    "mongodb",
    "express",
    "MERN-stack",
    "full-stack"
  ],
  "license": "ISC",
  "dependencies": {
    "body-parser": "^1.19.0",
    "cors": "^2.8.5",
    "express": "^4.17.1",
    "mongoose": "^5.12.12"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}
```