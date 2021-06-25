# THE **MERN** STACK — DEPLOYMENT

Now that the application is ready. let's deploy our application to the internet. We are going to use [Heroku](https://heroku.com/) which is one of best cloud platform to deploy Node.js applications. Create a free account in [heroku.com](https://id.heroku.com/login). For more information about using heroku refer the [documentation](https://devcenter.heroku.com/articles/getting-started-with-nodejs).

You can deploy to heroku in many ways, but in this tutorial we are going to use one of the easiest way: [Deploying with Git](https://devcenter.heroku.com/categories/deploying-with-git). Hope you have git installed in your computer.

## Frontend production build

For creating [production build](https://create-react-app.dev/docs/production-build/) of our front-end, run `npm run build` in the terminal in front-end directory. This create a `build` folder which contains all the files required for deployment. Our whole react application will become in the form of static HTML, CSS and JavaScript files. You can easily serve the build folder using express.

## Serving static files

Until now, we have run our application in two ports: `3000` for front-end and `5000` for back-end. You can serve the build files using the same port. Navigate to `index.js` and add the following code after the routes.

```js
// index.js
// ...
const path = require('path')

// deploying the application

if (process.env.NODE_ENV === 'production') {
	app.use(express.static('front-end/build'))

	app.get('*', (req, res) => {
		res.sendFile(path.resolve(__dirname, 'front-end', 'build', 'index.html'))
	})
}

```

We can use [express.static](https://expressjs.com/en/starter/static-files.html) middleware for serving static files. There is a drawback in this approach. Deploying a new version of the application requires generating new production build of the frontend. We can avoid this by generating the build during deployment. For deploying we should add few `scripts` in our `package.json`.

## Scripts

Install [concurrently](https://www.npmjs.com/package/concurrently) using `npm install concurrently` command. It helps in running two commands at the same time. We are going to run install and run build at the same time before deploying.

```diff
{
  // ...
  "scripts": {
    "start": "node index.js",
+		"server": "nodemon index.js",
+		"client-install": "npm install --prefix front-end",
+		"client": "npm start --prefix front-end",
-   "dev": "nodemon index.js"
+		"dev": "concurrently \"npm run server\" \"npm run client\" ",
+		"heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install --prefix front-end && npm run build --front-end client"
  }
  //...
}
```

## Heroku

After creating an account in heroku, install [heroku CLI](https://devcenter.heroku.com/articles/heroku-cli). You can check if heroku is installed by running the command `heroku --version`.

Run `heroku login`. It redirects you to the browser, after logging in come to the terminal.

Run `heroku create`. It will create a new heroku project with a random name.

Now you should add the environmental variables of our project. Heroku automatically sets the value for `PORT`. You should add `MONGO_URI` and `TOKEN_SECRET`.

```
heroku config:set MONGO_URI=<your mongo uri>
heroku config:set TOKEN_SECRET=<token>
```

Open your [heroku dashboard](https://dashboard.heroku.com), open the project you just created. Open the `Deploy` tab and follow the instructions.

![instructions]()

Make sure you have `.gitignore` with `node_modules` and `.env` file. 

After pushing the files, you will get the URL in the output. You can find mine [here](https://quiet-reef-77052.herokuapp.com/)