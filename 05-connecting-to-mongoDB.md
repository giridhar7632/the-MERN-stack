# THE **MERN** STACK — CONNECTING TO MONGODB

In this project, we use [MongoDB](https://www.mongodb.com/) as a database to store the data. MongoDB is a [document database](https://en.wikipedia.org/wiki/Document-oriented_database) that stores data in JSON format, which is easily accessible using JavaScript.

Basically you can install and run MongoDb on your own computer. However, we are going to use the cloud Mongo database service known as [MongoDB Atlas](https://www.mongodb.com/cloud/atlas). We can use the database directly from our JavaScript code using the official [MongoDB Node.js driver](https://mongodb.github.io/node-mongodb-native/) library or [Mongoose](http://mongoosejs.com/index.html). In this project, we will use the Mongoose library since it is easy to use compared to the official library.

## MongoDB Atlas

Go ahead and create an account at [https://cloud.mongodb.com]. Once you've created and logged into your account, create a [cluster](https://www.mongodb.com/basics/clusters). You can choose the provider and region whatever you want, I prefer using the recommended ones.

![Cluster AWS](/assests/09-mongo_aws.png)

Click on create cluster button and your Mongo cluster will be ready in lessthan 5 minutes. You should add the user who can use the cluster and an IP address from where to use the cluster.

Click on the `Connect` button. 

![connect](/assests/15-connect_to_db.png)

It asks you to add an IP address and a user. For IP address, click on Allow `Access from Anywhere` to keep it simple.

![IP allow access from everywhere](/assests/10-add_ip.png)

Then add the database user. Make sure you remember the password. It is very important in the next step to connect our application to MongoDB Atlas.

![MongoDB user](/assests/11-create_db_user.png)

Click on Choose a connection method and then choose the method "Connect your application".

![Connect your application](/assests/12-choose_conn_method.png)

This gives you the [Mongo URI](https://docs.mongodb.com/manual/reference/connection-string/), the connection string you need to establish connection with MongoDB. The URL looks like this:

```
mongodb+srv://User01:<password>@cluster0.mmchd.mongodb.net/myFirstDatabase?retryWrites=true&w=majority
```

Make sure you copy the string and save it somewhere. You should replace the `<password>` with your super secret password.

## Using Mongoose

Open the code editor and navigate to `index.js` where you left off creating a server.

Add the following code before defining the routes of your appplication.

```js
...

// connecting to mongodb atlas
const uri = 'mongodb+srv://User01:<password>@cluster0.mmchd.mongodb.net/myFirstDatabase?retryWrites=true&w=majority'
mongoose
  .connect(uri, {
    useNewUrlParser: true,
    useCreateIndex: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
  })
  .then(() => console.log('MongoDB connection is established successfully 🎉'))
  .catch(err => console.log(err))

...
```

This uses [`connect`](https://mongoosejs.com/docs/api/mongoose.html#mongoose_Mongoose-connect) method of Mongoose to establish connection with the URL you pass to it.

Run the command `npm run dev` and now you see the message 'MongoDB connection is established successfully 🎉' after it connects to database. If any error occurs, read the error message and try to troubleshoot the problem.

![connections](/assests/13-MongoDB_connection.png)

## Using environment variables

It is not very good to disply the secrets like our passwords in the code, especially when you are using version control like Git. [Environment variables](https://en.wikipedia.org/wiki/Environment_variable) are best solution of this solution. We can use variables according to the different environment like 'development', 'production' or 'test'.

Create two files named `.env` and `.gitignore`. The `.env` will contain all our environment variables.

```
PORT=
MONGO_URI=mongodb+srv://User01:<password>@cluster0.mmchd.mongodb.net/myFirstDatabase?retryWrites=true&w=majority
```

The PORT will be added at the time of deployment by the provider you use, so let's use it in our application, instead of hard-coded port number.

You can access the environment variables using the `dotenv` package we have installed before. You can read the environment variables using `process` module of Node.js.

Add the `dotenv` config at the top of your code.

```diff
/index.js

+require('dotenv').config()
// ...

-const port = 5000
+const port = process.env.PORT || 5000

// ...
-const uri = 'mongodb+srv://User01:<password>@cluster0.mmchd.mongodb.net/myFirstDatabase?retryWrites=true&w=majority'
+const uri = process.env.MONGO_URI

// ...
```

We should not push or upload this file containing our secrets to internet. So we should ignore this file from pushing it to version control using Git. You can use `.gitignore` file to serve this purpose.

```
node_modules
.env
```

You should not upload `node_modules` as well, it contains thousands of files which are neccessary to upload to git. 

You can see the progress of the project [here](https://github.com/giridhar7632/mern-notes-app/tree/5606c7631bc7e26b367aef2734bbf277b61b1d9b).