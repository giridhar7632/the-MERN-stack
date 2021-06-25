THE **MERN** STACK — MONGODB MODELS

## Schema

After establishing the connection with the database, we should define [Model](https://mongoosejs.com/docs/models.html) of our document using the [Schema](https://mongoosejs.com/docs/guide.html).

Schema defines the structure of the document. Actually, document databases like Mongo are schemaless, that means you can store any type of data in the same collection. We use mongoose to define the specific shape of the document in the collection.

In this project, we are going to create two collections of data. One for the *users* and other for the *notes* a user going to create. They will be like:

![schemas]()

Now create a folder named `models` in the root of your directory. We are going to keep the project structure simple and understandable. In the `models` create two files: one for *users* as `user.model.js` and other for *notes* as `note.model.js`. The following code is to define the schema for a user.

```js
const mongoose = require('mongoose')

const { Schema } = mongoose

const userSchema = new mongoose.Schema(
  {
    username: {
      type: String,
      required: true,
    },
    email: {
      type: String,
      required: true,
      unique: true,
    },
    password: {
      type: String,
      required: true,
    },
  },
  { timestamps: true }
)
```

This create the schema of a user with all items required. The data will be stored in the document as same as the schema. Our user schema contains a `username`, `email` which is unique and `password` in the form of `Strings`. Setting the `timestamps` to `true` will add `createdAt` and `updatedAt` fields to your schema.

## Model

Models are *constructor* functions compiled from Schema definitions. These are the models of documents that create new JavaScript objects to be stored in the database.

```js
const User = mongoose.model('User', userSchema)
module.exports = User
```

The `model` method creates the model of our data. It takes two arguments, the first argument is the singular name of the model's collection and second argument is the `schema`. Here, when you try to add data to the collection, it will be named as *users*.

## Creating and saving data

Now you can create new User object using the model and add it to database.

```js
const user = new User({
  username: 'user01',
  email: 'user01@gmail.com',
  password: '@user01_password'
})
```

You can save this new user object to database using `save` method of the `User` model.

```js
user.save().then(result => {
    console.log(`${result.username} added to database`)
  })
```

Here, after the data is saved to the database, the function provided to `then` gets called. Thus event-handler function logs the message with username to the console.

Now you can see the data saved to the database in MongoDB Atlas.

![data added to mongodb atlas]()

Learn more about [schema](https://mongoosejs.com/docs/guide.html) and [model](https://mongoosejs.com/docs/models.html). Try to create a model for the notes which is very similar to the user model we just created. Observe the [above figure](#Schema) for the fields to be in notes schema. At the end of this chapter, you can find the solution code for the *notes* model.

## Fetching data from the database

You can easily fetch the data stored in the database using the [`find`](https://mongoosejs.com/docs/api.html#model_Model.find) method of the `User` model.

```js
Note.find({}).then(result => {
  result.forEach(user => {
    console.log(user)
  })
})
```

The data will be returned as an array according to the parameter provided to the method. Here, the parameter is an empty object `{}`, we get all of the users stored in the database. We get one user logged since previously we added only one user.

> Try to add more users and fetch to log the data in the database to the console.

<details>
  <summary>Check the notes model here:</summary>

```js
const mongoose = require('mongoose')
const { Schema } = mongoose

const notesSchema = new Schema(
  {
    title: {
      type: String,
      default: 'Untitled',
    },
    content: {
      type: String,
      required: true,
    },
    user_id: {
      type: String,
      required: true,
    },
    date: {
      type: Date,
      default: new Date(),
    },
    name: {
      type: String,
      required: true,
    },
  },
  {
    timestamps: true,
  }
)

const Notes = mongoose.model('Notes', notesSchema)

module.exports = Notes
```

</details>

You can find the progress of project [here]().