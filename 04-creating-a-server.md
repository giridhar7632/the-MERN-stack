# THE **MERN** STACK — CREATING A SERVER

## Fundamentals of Web applications

Before writing code for our application, let's discuss some fundamental concepts about web development like what full-stack means and how a web application works.

A traditional web application has two major components: 

- Client — the user interface, typically web-accessing software available on internet-connected devices (usually a web browser like Chrome or Firefox).
- Server — the computers which serve web pages and applications to the client.

![Client-Server]()

The **Client** and **Server** communicate with each other using the [`HTTP`](https://developer.mozilla.org/en-US/docs/Web/HTTP) protocol (HyperText Transfer Protocol). These are like layers of web development. If you focus on creating the client-side, you are a front-end developer. Or if you develop some server-side stuff then you are a back-end developer. If you can do both well, it is full-stack development. 

This is what [MDN web docs](https://developer.mozilla.org/en-US/) say about what happens when you type a web address into your browser:

1) The browser goes to the server and finds the real address of the server that the website lives on.
2) The browser sends an [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) request message to the server, asking it to send a copy of the website to the client. This message, and all other data sent between the client and the server, are sent across your internet connection using [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol))/[IP](https://en.wikipedia.org/wiki/IP_address).
3) If the server approves the client's request, the server sends the client a "`200 OK`" message, which means "Of course you can look at that website! Here it is", and then starts sending the website's files to the browser as a series of small chunks called data packets.
4) The browser assembles the small chunks into a complete web page and displays it to you.

You can read more [here](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works).

There is another component for developing full-stack web applications, it is the database layer. The database contains all the data we use for the application. The server fetches the data from the database and serves it to the client.

For this project, we are going to create a server that responds to some commonly used requests: `GET`, `POST`, `PUT` and `DELETE`. These correspond to the basic operations using the database: `Read`, `Create`, `Update` and `Delete`. In short, we call them **CRUD** operations.

![CRUD]()

## Simple webserver

Navigate to `index.js` and add the following lines of code.

```js
const http = require('http')

const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello, World! 👋')
})

const port = 5000
app.listen(port)
console.log(`Server running on port ${port} 🚀`)
```

Save the file and run the command `npm start` in the terminal. This will create a simple web server running on the port you specify.

Once the application starts running, the following message is printed to the console:

```
Server running on port 5000 🚀
```

Now open the browser and visit [http://localhost:5000](http://localhost:5000). You can see the message sent to the browser.

This is a simple server created using the Node.js `http` module.

```js
const http = require('http')

const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello, World! 👋')
})
```

The first line imports the built-in `http` module of node.js. The `createServer` method of the http module creates a new web server every time an HTTP request is made to the server(http://localhost:5000).

```js
app.listen(port)
```

This line makes the server listen to HTTP requests sent to port 5000.

Now if your applications have more than one endpoint, it will become complicated to implement using the `http` module. Many libraries have been developed to make this process easier for us. One of the most popular libraies for creating a web server easily is [express](https://expressjs.com/).

## Express server

```js
const express = require('express')
const app = express()
const port = 3000

app.get('/', (request, response) => {
  response.send('Hello, World!')
})

app.listen(port, () => {
  console.log(`Server running on port ${PORT} 🚀`)
})
```

When you run the application, it starts a web server at the port you specify. The app responds with the message when you make a request to the server.

Express makes things way easier than using a simple `http` module. At the very last express is just a library that was built upon using the `http` module of Node.js.

```js
const express = require('express')
const app = express()
```

The above lines of code import `express` and initiate the express application and assign it to a variable `app`.

Next we should use the method we required according to HTTP request to that route. 

```js
app.get('/', (request, response) => {
  response.send('Hello, World!')
})
```

The above code handles the HTTP `GET` request made to applications's `/` root. The event-handler function will be executed. It accepts two parameters: [`request`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#http_requests) - contains all the information of the HTTP request, and [`response`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#http_responses) - the response to be handled.

The response sends a status of the request, in our case `200` means OK. There are many other status codes, you don't have to remember every code but just have an understanding of them.

Then we use `send` method to send a string as a response. You can also send [JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON) objects using the `json` method.

Learn more about [HTTP status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) here and try to add more endpoints and send different types of data as a response.