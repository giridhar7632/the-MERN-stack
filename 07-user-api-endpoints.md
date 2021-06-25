# THE **MERN** STACK — USER AUTHENTICATION

Now let's start creating the API endpoints for Authentication for users. Create two new folders called `routes` and `controllers`. The `routes` folder is to store the entire routes required for our application and `controllers` folder is to write the functions of the routes.

## API endpoints for user

|URL|HTTP method|functionality|
|----|----|----|
|`/users/register`|POST|register a new user based on the request data|
|`/users/login`|POST|to log in a user|
|`/users/verify`|GET|to verify if the token of the user is valid or not|

## User Router

Create a file `userRouter.js` in `routes` folder.

```js
// userRouter.js

const router = require('express').Router()
const userCtrl = require('../controllers/users.js')

// 1. Register a user
router.post('/register', userCtrl.registerUser)

// 2. Log in a user
router.post('/login', userCtrl.loginUser)

module.exports = router
```

Basically, we send the data of the HTML form to the endpoint to register or login a user. To keep our code clean and readable, we create the route functions in `controllers` folder and import them into the Router files just like in the above code. 

You should not forget to add base route for the routes you create. Import these routes into and index.js file and use the method `use` to create the base routes.

```js
// index.js

// ...
const userRouter = require('./routes/userRouter.js')
// ...

app.use('/users', userRouter)

// ...
```

## User Controllers

Create a new file in `controllers` folder and name it as `userCtrl.js`. Let's create the `registerUser` and `loginUser` functions in `userCtrl`. We need the `User` model we created before and `bcrypt` to [hash](https://auth0.com/blog/hashing-in-action-understanding-bcrypt/) the password. We should not save the passwords as it is. They are the secrets of our clients so we shoud save the encrypted passwords in the database.

```js
// userCtrl.js

const User = require('../models/user.model.js')
const bcrypt = require('bcrypt')

const registerUser = (req, res) => {
  res.send('Registered user')
}

const loginUser = (req, res) => {
  res.send('User logged in')
}

module.exports = { registerUser, loginUser }
```

## Register a User

For registering a user, we accept data(like username, email and password) from the user and then:

1) If the email already exists, we return with status code `400` and message `Email already exists`.
2) Else, we encrypt the password using `hash` method of bcrypt.
3) And then we create a new user in the database with the username, email and hashed password.

```js
// ...

const registerUser = async (req, res) => {
  try {
    const { username, email, password } = req.body

    // get the user by email
    const user = await Users.findOne({ email: email })

    // 1. Check if email already exists
    if (user)
      return res
        .status(500)
        .json({ msg: 'Email already exists.', type: 'error' })

    // 2. Encrypt password
    const passwordHash = await bcrypt.hash(password, 10)

    // 3. Create a user in database
    const newUser = new User({
      username: username,
      email: email,
      password: passwordHash,
    })

    await newUser.save()    // saving user to database
    res.json({ msg: 'Registered Successfully', type: 'success' })
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}  

//...
```

> Note: the `type` field we send in the message is purely for styling the notification in the front-end.

## User Login

It is very similar to registering the user. Here, we accept data of email and password from the user. Then:

1) Check if the user exists
2) If user doesn't exist, we return with a status of `500` and warning message saying `User does not exist`.
3) Else we check if it is the correct password. We use the `compare` method to match the passwords.
    - If password doesn't match we return a error message `Incorrect password`.
4) If login was success, we send the success message.

```js
// ...

const loginUser = async (req, res) => {
  try {
    const { email, password } = req.body

    // 1. check if user exists
    const user = await Users.findOne({ email: email })

    // 2. If user dosen't exists
    if (!user)
      return res
        .status(500)
        .json({ msg: 'User does not exist.', type: 'warning' })

    // 3. Check for password match
    const isMatch = await compare(password, user.password)
    if (!isMatch)
      return res
        .status(500)
        .json({ msg: 'Incorrect password.', type: 'error' })

    // 4. if login success, create a token
    res.json({ token, msg: 'Sign in Successful.', type: 'success' })
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}

// ...
```

## Token authentication

If the user tries to login, we should keep the user logged in for sometime until he/she wants to logout. This functionality can be implemented using [JSON web tokens](https://jwt.io/). We can create a JSON web token using the user's id, username and `sign` method of [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken) library. For making the token unique, it is created using a secret that belongs to you only. Create a new environment variable and assign your secret.

```js
const jwt = require('jsonwebtoken')

// ...

const registerUser = async (req, res) => { ...
}

const loginUser = async (req, res) => {

  // ...

  // 4. if login success, create a token
    const userForToken  = { id: user._id, name: user.username }
    const token = jwt.sign(userForToken , process.env.TOKEN_SECRET, {
      expiresIn: '1d',
    })

    res.json({ token, msg: 'Sign in Successful', type: 'success' })
}

// ...
```

Here, `expiresIn` option in `sign` method is the time until the token expires(`1d` = 1 day).

## Verifying Token

We use the token to verify and get the data of the user. To serve this purpose, let's create a middleware function. Create a new folder and name it as *middleware*. Create a file named `auth.js`.

In the *middleware* function, we need to access the `Authorization` header of the request which contains the JWT token. Then:

1) If there is no token, we return with status `400` and message `Invalid Authentication`.
2) Else, we verify the token using `verify` method of `jsonwebtoken`.
    - If, the token is invalid, we return error message of `Invalid token`.
    - Else, we create the `user` field in request with the data from the token.
3) Then the next function in the request-response cycle will be executed.

```js
const jwt = require('jsonwebtoken')

const auth = (req, res, next) => {
  try {
    const token = req.header('Authorization')
    if (!token) return res.status(400).json({ error: 'Invalid Authentication' })

    jwt.verify(token, process.env.TOKEN_SECRET, (err, user) => {
      if (err) return res.status(400).json({ error: 'Token not valid.' })

      req.user = user
      next()
    })
  } catch (error) {
    return res.status(500).json(error)
  }
}

module.exports = auth
```

Then create a new route `/verify` in the `userRouter.js` file. This needs to import the *middleware* we created. If the token exists, the `verifyUser` function will be executed.

```js
// ...

// 3. Verify token
router.get('/verify', userCtrl.verifyUser)

// ...
```

Navigate to `userCtrl.js` and start writing the `verifyUser` function.

For verifying the token, we extract the token from the `Authorization` header and then:

1) If there is no token, the user is not verified, return false
2) Then verify the token, using `verify` method of `jsonwebtoken`.
3) Check if user exists in our database
    - If doesn't exist, return false
    - Else, return true.

```js
// ...

const verifyUser = (req, res) => {
  try {
    const token = req.header('Authorization')

    // 1. if there is no token
    if (!token) return res.send(false)

    // 2. Verify the token
    jwt.verify(token, process.env.TOKEN_SECRET, async (err, verified) => {
      if (err) return res.send(false)

      // 3. if the user exists in our database
      const user = Users.findById(verified.id)
      if (!user) return res.send(false)

      return res.send(true)
    })
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}

// ...
```

> Note: We should test each and every endpoint we create. If any error occurs, we should troubleshoot at the same time. In this tutorial, I have dedicated a seperate chapter for testing the endpoins we create using Postman.

The project so far will be like [this]().
