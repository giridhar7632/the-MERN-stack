# THE **MERN** STACK — INTRODUCTION

In modern days, the influence of JavaScript is multiplying. In the olden days, JavaScript only runs inside the browser, and you have to choose a different language like [PHP](https://www.php.net/) or [Python](https://python.org) to build the server-side of the web applications. Nowadays, many developers developed various technologies to help you create a complete full-stack web application only using JavaScript, including both client-side and server-side. The **MERN** stack is a JavaScript stack with the only JavaScript to work with a specific technology without another programming language.

Each letter in the **MERN** stack represents a technology.

- M — MongoDB - a database solution
- E — Express - a Node.js framework
- R — React - a front-end library
- N — Node - a JavaScript run-time environment

We'll combine the powers of React with the server-side environment developed using Node, Express, MongoDB to create unique full-stack web applications.

## React

[React](https://reactjs.org) is the most popular front-end framework used to create a blazing-fast dynamic user interface using JavaScript. It was released to the world by Facebook in 2013. It was the leading technology of most popular web and mobile applications such as Facebook and Instagram. It makes the development of the user interface easy by dividing it into several reusable components. It can create dynamic web applications using the state mechanism, updating and rendering the suitable UI component when a specific data changes. It uses the concept of **virtual DOM**, which is different from the browser's DOM, to make the applications faster than ever. It makes it easy for developers to create and maintain reusable, dynamic, and complex user interfaces.

In this tutorial, you use [Chakra UI](https://chakra-ui.com/) for styling the front-end. Chakra UI is a simple, modular and accessible component library that gives you the building blocks you need to build your React applications. We can focus on the server-side code instead of struggling to make the client-side more effective.

Learn more about front-end development using React [here](https://tutorialspoint.com/reactjs/).

## Node

Node.js is a JavaScript run-time environment. It makes JavaScript code accessible outside the browser. The developer of Node.js built it on top of Google Chrome's [V8](https://v8.dev/) JavaScript engine, and its primary use is to create server-side applications. Node.js is simple, extremely fast, and handles programs asynchronously. Node.js provides an event-driven architecture capable of asynchronous, non-blocking I/O, making it more quickly than other programming languages. Node has a vast collection of [npm](https://npmjs.org) (**N**ode **P**ackage **M**anager) packages, making your development process more accessible. There are more than 500,000 open source packages you can freely use. You have to install React, Express into your project using npm. Instead of [npm](https://npmjs.org), you can also use [yarn](https://yarnpkg.com/) which is also a node package manager.

Learn more about Node.js [here](https://www.tutorialspoint.com/nodejs/).

## Express

Express is a framework of Node.js, which makes the development of servers more effortless than ever. It extends the features of Node.js and provides an easy-to-use framework for building a powerful server with even fewer lines of code. It makes dynamic routing easier, and you can add any compatible middleware functions to extend its functionality. Middleware is a function with access to the request and response of an HTTP request and the following function in the request-response cycle.

Learn more about Express.js [here](https://www.tutorialspoint.com/expressjs/).

## MongoDB

MongoDB is a cross-platform, document-oriented, No-SQL database. It stores data in flexible JSON-like documents. You can work efficiently with a large amount of data using JavaScript easily in JSON (**J**ava**S**cript **O**bject **N**otation) format. It is a powerful and easy-to-use database engine that can integrate with an express environment very quickly. However, you can replace the database solution with another alternative database engine. But it is not MERN stack anymore. Because of its features and the other three technologies, we use MongoDB as our database.

Learn more about MongoDB [here](https://www.tutorialspoint.com/mongodb/).

As all these technologies are JavaScript-based and integrate well together, we use them to build powerful full-stack web applications that can serve our industry and real-world requirements.

In this tutorial, you learn how to integrate these technologies by building a full-stack web application **Notes Taking App**. You'll learn how to implement web authentication using JSON Web Tokens ([jwt](https://jwt.io/)). You'll learn how to implement essential **CRUD** functions using a server and a database. Without any delay, let's start setting up the development environment for the project.
