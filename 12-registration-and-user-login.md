# THE **MERN** STACK — REGISTRATION AND USER LOGIN

Now we have a react application to render data inside the browser and a server to get data. Let's connect the front-end to the back-end.

You can use JavaScript built-in [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) for making HTTP requests to the server. Instead, we are going to use `axios` which makes sending requests a lot easier than the `fetch` method. Keep the server running the whole time.

## Proxy

[Proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/) is used in the development environment to make communication between server and client easier. We should add the `proxy` field in `package.json` of our react application. It redirects the requests made to the base URL to the proxy URL, which is our backend.

```diff
// package.json
{
  // ...
+ "proxy": "http://localhost:5000"
}

```

## Sending data to the server

We use `axios.post` method to send data to the server. Navigate to `/src > /components > /notes > Login.js`. Let's add the registration functionality. 

### Setting up toast

Toast is something that just pops up with some message. We will use the `useToast` method from chakra UI. Refer to the documentation for more information about [toast](https://chakra-ui.com/docs/feedback/toast).

```js
// src/components/Login.js

import React, { useState, useRef } from 'react'
import { useToast } from '@chakra-ui/react'
// ...

const Login = () => {
  // ...
  const toast = useToast()
  const toastIdRef = useRef()
  const addToast = (text, type) => {
    toastIdRef.current = toast({
      title: `${text}`,
      status: `${type}`,
      isClosable: true,
      duration: 3000,
    })
  }

  // ...
}
```

We created a function `addToast` so that we can use it multiple times with custom `text` and `type`. If you remember, the response sent by the server contains two fields: `msg` and `type`. The `type` field is for the type of toast.

## Register User

On submitting the Registration form, we should send the data of the user for registration.

For registering a user, 
1) We should send the form data to the server using HTTP POST.
2) Then we should clear the `user` state and form for another registration.
3) We should display the success message if everything is completed and got the response, else display the error message if occurs.

```js
// src/components/Login.js

import React, { useState, useRef } from 'react'
import { useToast } from '@chakra-ui/react'
// ...

const Login = () => {
  // ...
  const registerUser = async e => {
    e.preventDefault()
    try {
      // 1) sending data to server
      const res = await axios.post('/users/register', {
        username: user.name,
        email: user.email,
        password: user.password,
      })
      // 2) Resetting the state
      setUser({ name: '', email: '', password: '' })

      // 3) Displaying success message
      addToast(res.data.msg, res.data.type)
      setIsRegistered(true)
    } catch (error) {
      error.response.data.msg &&
        addToast(error.response.data.msg, 'error')
    }
  }

  return (
    // ... 
    // Registration form
    <form onSubmit={registerUser}>
      <Heading as="h2" size="lg" textAlign="center" mb={6}>
        Register
    // ... 
  )
  // ...
}
```

Now try to register the user, you should get redirected to the `Login` form and get a message saying `Registered Successfully`. If you try to register again with the same email, you should see an error message saying `Email already exists`. These are the responses coming from the server.

## Login a user

Similar to registration, you should send the credentials as the data to the server and retrieve the token for storing it in `localStorage` of the browser for future use. Also set the `isLogin` to true, which renders the notes component.

```js
// src/components/Login.js

// ...

const Login = ({ setIsLogin }) => {
  // ...
  const userLogin = async e => {
    e.preventDefault()
    try {
      // sending data to the server
      const res = await axios.post('/users/login', {
        email: user.email,
        password: user.password,
      })
      // Resetting the state
      setUser({ name: '', email: '', password: '' })

      // Displaying success message
      addToast(res.data.msg, 'success')

      // storing token in local storage
      localStorage.setItem('userToken', res.data.token)
      setIsLogin(true)
    } catch (error) {
      error.response.data.msg &&
        addToast(error.response.data.msg, 'error')
    }
  }

  return (
    // Login form
    <form onSubmit={userLogin}>
      <Heading as="h2" size="lg" textAlign="center" mb={6}>
        Login
    // ... 
  )
  // ...
}
```

Now if you try to login in, you will be redirected to the notes component. If any error occurs, you should receive a message saying the error. When you open Dev tools, you can see the token created in `LocalStorage`.

![token saved in localstorage](/assests/36-userToken.png)

## Next time login

If you have once logged in, you can stay log in until the token expires, when you open the application another time, it should check for the token and redirect you to the notes component, without asking to login.

## The Effect Hook

The `useEffect()` hook accepts a function as an argument. The function runs when the component is first rendered, and on every subsequent rerender/update. React first updates the DOM, then calls any function passed to `useEffect()` without blocking the UI. We should check whether there is a token in the `localStorage` and whether it is valid or not.

```js
// src/App.js

import React, { useState, useEffect } from 'react'
// ...
const App = () => {
  // ...
  useEffect(() => {
      const checkLogin = async () => {
        // getting token from local store
        const token = localStorage.getItem('userToken')
        if (token) {
          // verifying the token
          const verified = await axios.get('/users/verify', {
            headers: { Authorization: token },
          })
          // changing the state of isLogin
          setIsLogin(verified.data)
          // if token is not valid
          if (verified.data === false) return localStorage.clear()
        } else {
          // if token is missing
          setIsLogin(false)
        }
      }
      checkLogin()
    }, [])
    
    // ...
}

```

The `checkLogin` function is called after rendering the UI. It checks local storage for token, if there is a token it sends a GET request to the route `/users/verify`. It responds `true` if the token is valid or `false` if the token is invalid. For sending a request, we can specify the headers as an option in the request. If the token is valid, we set the `isLogin` to true which redirects you to the notes component without login.

## Log out

To log out from the account, you can just delete the token stored in local storage and set the `isLogin` to false.

```js
// src/components/notes/Header.js

// ...
const Header = ({ setIsLogin }) => {
  const logOut = () => {
    localStorage.clear()
    setIsLogin(false)
  }

  return(
    // ...
    <Button onClick={logOut} mx={3} variant="solid" size="md">
      Logout
    </Button>
    // ...

// ...
```

After logging in you can log out by clicking the `Logout` button in the header.

At this point, our application has a fully functioning authentication system. The approach we used is a bit risky. Storing tokens in local storage might contain a security risk if the application has a security vulnerability that allows [Cross-Site Scripting](https://owasp.org/www-community/attacks/xss/) (XSS) attacks. You should store the token in [cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict_access_to_cookies) to restrict access using JavaScript. It reduces the risk to some extinct.

You can find the progress of the application [here](https://github.com/giridhar7632/mern-notes-app/tree/897feecefbf3a2d7a770d82384bb04548562872d).
