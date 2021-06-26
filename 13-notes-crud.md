# THE **MERN** STACK — NOTES CRUD IMPLEMENTATION

## Getting notes from the server

We can get all the data from the server using the `get` method of axios. We should add the header `Authorization` with the user token to get the notes.

Navigate to `Home.js` and create a function `getNotes`. When the component renders, we should get the token from the local storage and send the HTTP GET request to the server. Then set the state(`notes`) to the array we got from the database.

```js
// src/components/notes/Home.js

import React, { useState, useEffect } from 'react'
import axios from 'axios'
// ...

const Home = () => {
  const [notes, setNotes] = useState([])
  const [token, setToken] = useState('')

  const getNotes = async token => {
    // sending get request
    const res = await axios.get('api/notes', {
      headers: { Authorization: token },
    })
    setNotes(res.data)
  }

  useEffect(() => {
    // getting token from the local storage
    const token = localStorage.getItem('userToken')
    setToken(token)
    if (token) {
      getNotes(token)
    }
  }, [])

  // ...
}
```

## Sending notes to the server

Now let's implement the functionality to create a new note in the database using the front-end. Navigate to the `CreateNote.js` component. Create the `addToast` function just like we did in the `Login.js` component. Then we should create a function to send the note to the server. Only a logged-in user with a valid token can create the note. So we have to send the token with the POST request in the `Authorization` header.

```js
// src/components/notes/CreateNote.js

import React, { useRef, useState } from 'react'
import { useHistory } from 'react-router-dom'
// ... 

const CreateNote = () => {
  // ...
  const history = useHistory()

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

  const createNote = async e => {
    e.preventDefault()
    try {
      const token = localStorage.getItem('userToken')
      if (token) {
        const { title, content, date } = note
        const newNote = { title, content, date }

        const res = await axios.post('/api/notes', newNote, {
          headers: { Authorization: token },
        })
        addToast(res.data.msg, res.data.type)

        return history.push('/')
      }
    } catch (error) {
      window.location.href = '/'
    }
  }

  return (
    <div>
      <Box w="30vw">
        <Heading my={4}> Create Note</Heading>
        <form onSubmit={createNote}>
        // ...
// ...
```

The `createNote` function sends the form data to the server, displays the success message, and returns to the home page. The [useHistory](https://reactrouter.com/web/api/Hooks/usehistory) hook of react-router-dom facilitates returning to the home page. If any error occurs, it just returns to the home page. You should add the `onSubmit` event handler to the form.

Try to add a note, you can see that the note gets added to the database and is displayed on the screen.

## Updating notes in the server

You can easily update the notes using the HTTP PUT request. The `EditNote` component first gets the specific note from the server and populates the form with that data. After you completed editing the data in the form it sends a PUT request to the same endpoint which updated the data in the server.

```js
// src/components/notes/EditNote.js

import React, { useEffect, useRef, useState } from 'react'
import axios from 'axios'
import { useHistory } from 'react-router-dom'
// ...

const EditNote = ({ match }) => {
  // ...
  const history = useHistory()

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

  // gets the data from the server
  useEffect(() => {
    const getNote = async () => {
      const token = localStorage.getItem('userToken')

      // accessing the id from the URL using match 
      if (match.params.id) {
        const res = await axios.get(`/api/notes/${match.params.id}`, {
          headers: { Authorization: token },
        })
        console.log(res.data.date)
        setNote({
          title: res.data.title,
          content: res.data.content,
          date: new Date(res.data.date).toLocaleDateString(),
          id: res.data._id,
        })
      }
    }
    getNote()
  }, [match.params.id])

  const editNote = async e => {
    e.preventDefault()
    try {
      const token = localStorage.getItem('userToken')
      if (token) {
        const { title, content, date, id } = note
        const newNote = { title, content, date }

        const res = await axios.put(`/api/notes/${id}`, newNote, {
          headers: { Authorization: token },
        })
        addToast(res.data.msg, res.data.type)
        history.push('/')
      }
    } catch (error) {
      window.location.href = '/'
    }
  }

  return (
    <Box w="30vw">
      <Heading my={4}> Edit Note</Heading>
      <form onSubmit={editNote}>...</form>
      // ...
}
// ...
```

## Deleting notes in the server

If you click the delete button on the note, it will get deleted from the server. You have to just send the DELETE request to the server with the notes id. After deleting, we again send the get request, to get the new array of notes.

```js
// src/components/notes/Home.js

// ...
const Home = () => {
  // ...

  const deleteNote = async id => {
    try {
      if (token) {
        await axios.delete(`api/notes/${id}`, {
          headers: { Authorization: token },
        })
        getNotes(token)
      }
    } catch (error) {
      window.location.href = '/'
    }
  }
// ...
}

```

That is it. We completed building a notes app using the MERN stack. Check the progress [here](https://github.com/giridhar7632/mern-notes-app/tree/5933b591540a89b840a059ceed8385a9908ee386).