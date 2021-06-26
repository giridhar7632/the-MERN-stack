# THE **MERN** STACK — REACT FRONT-END PART 1

In this chapter, we are gonna create the front-end of our application. We will use [React](https://reactjs.org), as I said earlier in the introduction, it is a JavaScript library that helps to create interactive and dynamic user interfaces. One of the main reasons I love React was it is component-based, every element can be considered as a component. Components make our code pretty reusable, flexible, and scalable.

## Basic concepts of React

- JSX — it is the extension of JavaScript syntax to write HTML inside JavaScript. It sounds weird but it's pretty much the same as HTML but you can write JavaScript inside braces `{}`.
- Components — React is all about components. A react component is anything that contains some code, data and can be used anywhere. There are two types of components: `Class`-based components and `Functional` components. In this project, we are gonna use only functional components and hooks(for state management) because of their benefits.
- Props — it is the short name for *properties*, the way to pass data between the components. These are the input *arguments* that you can use inside the component.
- State — it is the data of our application. It can be used for implementing dynamic functionality in our application. 

You'll get used to these as we get going through the project.

The structure of our project will be:

1) If the user is not logged in, it will display the login page.
2) If you don't have an account, you can create an account on the register page.
3) After login, it's gonna display the notes.
4) You can navigate to `/create` to create a new note.
5) You can update the note on `/edit` page.
6) You can delete any note by clicking the `delete` button on the note.

The `front-end` folder structure will be big in number but these are the only file you have to concentrate on:

```
front-end/
├── node_modules/
│
├── public/
│   └── index.html
│
├── src/
│   ├── index.js
│   ├── App.js
│   └── ColorModeSwitcher.js
│
├── .gitignore
├── package.lock.json
└── package.json
```

The `index.html` in the *public* directory will be the page that is displayed in the browser. We are not going to touch any other directory except the `src` folder. All the code we write in the `src` folder will be rendered inside the `<div>` element with id `root` in the HTML file. Under the hood, `react-dom` will do this for us.

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))

```

The `App` component will be the home page of our application. It should contain everything you want to render in the browser. You can see some other stuff inside `index.js` if you have installed the chakra-UI template. It's just for enabling the working of Chakra UI and nothing more. It won't affect the way how the basic React application works.

## App component

As discussed, it is the home page and this will be your first react component. Clear all the code in the `App.js` file and let's add our code.

```js
// src/App.js

import React from 'react'

const App = () => {
  return <div>Hello, World!! 👋</div>
}

export default App

```

Above is the basic react functional component. It prints `Hello, World!!` in the browser. Read the basics of JSX [here](https://reactjs.org/docs/introducing-jsx.html).

## Chakra UI

We are going to use Chakra UI for styling the application. It provides accessible components which can be imported into our files and use just like any other react components. It is a good idea to refer to the [documentation](https://chakra-ui.com/docs/getting-started) along the code.

For Chakra UI to work correctly, we should set up the `ChakraProvider` at the main component(`App.js`) of our application.

```js
import React from 'react'
// 1. import `ChakraProvider` component
import { ChakraProvider } from "@chakra-ui/react"
const App = () => {
  // 2. Use at the root of your app
  return (
    <ChakraProvider>
      <div>Hello, World!! 👋</div>
    </ChakraProvider>
  )
}
```

## Using State and Props

Here, we should display the `Notes` component if the user is logged in otherwise we should display the login component. This is called [conditional rendering](https://reactjs.org/docs/conditional-rendering.html) So we are going to add `isLogin` as the state, which changes according to the user. We use the [useState](https://reactjs.org/docs/hooks-state.html) hook to handle our state. We send the data as a [prop](https://reactjs.org/docs/components-and-props.html#props-are-read-only) to another component to update the state.

Create a folder inside the `src` folder and name it as `components`. We'll create all our components inside this folder. Create the `Notes.js` and `Login.js`. You can use the [openchakra.app](https://openchakra.app/) for designing the pages and copy the code from them.

## App.js

```jsx
// src/App.js

import React, { useState } from 'react'    // importing useState hook
import { ChakraProvider, Flex, theme } from '@chakra-ui/react'

// importing components we created
import Notes from './components/Notes'
import Login from './components/Login'


const App = () => { 
  const [isLogin, setIsLogin] = useState(false)    // using state variables

  return (
    <ChakraProvider theme={theme}>
      <Flex p={8} h="100vh" flexDirection="column" textAlign="center">
        <Flex maxW="100%" justifyContent="center" mt={4}>
          {isLogin 
            ? <Notes setIsLogin={setIsLogin} /> 
            : <Login setIsLogin={setIsLogin} />
          }
        </Flex>
      </Flex>
    </ChakraProvider>
  )
}

export default App
```

> Note: We used the [ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) (`condition ? exprIfTrue : exprIfFalse`) which is short from `if-else`.

## Login.js

On the login page, we should create two forms, one for login and another for registering. We will switch between these components using a state `isRegistered`. By default, we display the Login form and set `isRegistered` to true. The user can switch between these two by clicking the link to register. You can use `event-handlers` such as `onClick`, `onChange` just like in JavaScript. But you cannot select an element using the [DOM selectors](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Locating_DOM_elements_using_selectors) because there is no DOM in React, it uses a virtual DOM for rendering.


```jsx
// src/components/Login.js

import React, { useState } from 'react'
import { Button } from '@chakra-ui/button'
import { FormControl, FormLabel } from '@chakra-ui/form-control'
import { Input, InputGroup, InputRightElement } from '@chakra-ui/input'
import { Flex, Heading, Link, Stack, Text } from '@chakra-ui/layout'

const Login = ({ setIsLogin }) => {

  // form inputs
  const [user, setUser] = useState({
    name: '',
    email: '',
    password: '',
  })
  const [show, setShow] = useState(false)
  const [isRegistered, setIsRegistered] = useState(true)

  // event-handlers
  const handleChange = e => {
    const { name, value } = e.target
    setUser({ ...user, [name]: value })
  }

  // to hide or show the password
  const handlePassword = () => setShow(!show)

  // to swtich between login and register forms
  const handleResigter = () => setIsRegistered(!isRegistered)
  const handleLogin = () => setIsRegistered(!isRegistered)

  return (
    <Stack direction="column">
      {isRegistered ? (
        <Flex
          className="login"
          p="6"
          flexDirection="column"
          justifyContent="center"
        >
          <form>
            <Heading as="h2" size="lg" textAlign="center" mb={6}>
              Login
            </Heading>
            <FormControl id="email" isRequired>
              <FormLabel>Email</FormLabel>
              <Input
                type="email"
                name="email"
                value={user.email}
                onChange={handleChange}
                placeholder="Email"
              />
            </FormControl>
            <FormControl id="password" mt="2" isRequired>
              <FormLabel>Password</FormLabel>
              <InputGroup size="md">
                <Input
                  pr="4.5rem"
                  type={show ? 'text' : 'password'}
                  name="password"
                  value={user.password}
                  onChange={handleChange}
                  placeholder="Enter password"
                />
                <InputRightElement width="4.5rem">
                  <Button h="1.75rem" size="sm" onClick={handlePassword}>
                    {show ? 'Hide' : 'Show'}
                  </Button>
                </InputRightElement>
              </InputGroup>
            </FormControl>
            <Button mt={6} type="submit" colorScheme="purple" variant="solid">
              Login
            </Button>
          </form>
          <Text mt={6}>
            Don't have an account?{' '}
            <Link color="purple.500" onClick={handleResigter}>
              Register now
            </Link>
          </Text>
        </Flex>
      ) : (
        <Flex
          className="register"
          p="6"
          flexDirection="column"
          justifyContent="center"
        >
          <form>
            <Heading as="h2" size="lg" textAlign="center" mb={6}>
              Register
            </Heading>
            <FormControl id="username" isRequired>
              <FormLabel>Username</FormLabel>
              <Input
                value={user.name}
                name="name"
                onChange={handleChange}
                placeholder="Username"
              />
            </FormControl>
            <FormControl id="email" mt="2" isRequired>
              <FormLabel>Email</FormLabel>
              <Input
                type="email"
                name="email"
                value={user.email}
                onChange={handleChange}
                placeholder="Email"
              />
            </FormControl>
            <FormControl id="password" mt="2" isRequired>
              <FormLabel>Password</FormLabel>
              <InputGroup size="md">
                <Input
                  pr="4.5rem"
                  type={show ? 'text' : 'password'}
                  value={user.password}
                  name="password"
                  onChange={handleChange}
                  placeholder="Enter password"
                />
                <InputRightElement width="4.5rem">
                  <Button h="1.75rem" size="sm" onClick={handlePassword}>
                    {show ? 'Hide' : 'Show'}
                  </Button>
                </InputRightElement>
              </InputGroup>
            </FormControl>
            <Button mt={6} type="submit" colorScheme="purple" variant="solid">
              Register
            </Button>
          </form>
          <Text mt={6}>
            Already have an account?{' '}
            <Link color="purple.500" onClick={handleLogin}>
              Sign in
            </Link>
          </Text>
        </Flex>
      )}
    </Stack>
  )
}

export default Login
```

> Note: You can learn more about creating forms in react [here](https://reactjs.org/docs/forms.html).

Chakra UI components used in creating Login component:

- [Button](https://chakra-ui.com/docs/form/button)
- [FormControl](https://chakra-ui.com/docs/form/form-control), FormLabel
- [Input](https://chakra-ui.com/docs/form/input), InputGroup, InputRightElement
- [Flex](https://chakra-ui.com/docs/layout/flex), [Stack](https://chakra-ui.com/docs/layout/stack), [Link](https://chakra-ui.com/docs/navigation/link), [Text](https://chakra-ui.com/docs/typography/text), [Heading](https://chakra-ui.com/docs/typography/heading).

Save all the files and run the react application.

```
cd front-end
npm start
```

Then you can see the following login form in the browser. If you clock the link `Register now`, you can see the Registration form.

![Login form](/assests/34-login_form.png)

![Registration form](/assests/35-register_form.png)
