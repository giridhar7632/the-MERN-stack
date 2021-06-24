# THE **MERN** STACK — REACT FRONT-END PART 2

In this chapter let's create the components for displaying notes. Create a new folder `notes` inside the components folder and add four files: 1) `Header.js`, 2) `CreateNote.js`, 3) `EditNote.js` and 4) `Home.js`.

## CreateNote.js

This component is just a simple form to input the notes from the user.

```jsx
// src/components/notes/CreateNote.js

import React, { useState } from 'react'
import { Button } from '@chakra-ui/button'
import {
  FormControl,
  FormLabel,
  Input,
  Textarea,
  Heading,
  Box,
} from '@chakra-ui/react'

const CreateNote = () => {
  const [note, setNote] = useState({
    title: '',
    content: '',
    date: '',
  })

  const handleChange = e => {
    const { name, value } = e.target
    setNote({ ...note, [name]: value })
  }

  return (
    <div>
      <Box w="30vw">
        <Heading my={4}> Create Note</Heading>
        <form>
          <FormControl>
            <FormLabel>Title</FormLabel>
            <Input
              name="title"
              value={note.title}
              onChange={handleChange}
              placeholder="Untitled"
            />
          </FormControl>

          <FormControl mt={4} isRequired>
            <FormLabel>Content</FormLabel>
            <Textarea
              name="content"
              value={note.content}
              onChange={handleChange}
              placeholder="Content of the note"
            />
          </FormControl>

          <FormControl mt={4}>
            <FormLabel>Date</FormLabel>
            <Input
              name="date"
              value={note.date}
              onChange={handleChange}
              type="date"
            />
          </FormControl>
          <Button type="submit" colorScheme="purple" mt={3}>
            Add Note
          </Button>
        </form>
      </Box>
    </div>
  )
}

export default CreateNote

```

## EditNote.js

It is pertty similar to the one above but we get the `id` of note as a `prop` for the component. It will be filled with the data pre-existing in the database, we have to change and submit it.

```jsx
// src/components/notes/EditNote.js

import React, { useState } from 'react'
import { Button } from '@chakra-ui/button'
import {
  FormControl,
  FormLabel,
  Input,
  Textarea,
  Heading,
  Box,
} from '@chakra-ui/react'

const EditNote = ({ noteId }) => {
  const [note, setNote] = useState({
    title: '',
    content: '',
    date: '',
    id: '',
  })

  const handleChange = e => {
    const { name, value } = e.target
    setNote({ ...note, [name]: value })
  }

  return (
    <Box w="30vw">
      <Heading my={4}> Edit Note</Heading>
      <form>
        <FormControl>
          <FormLabel>Title</FormLabel>
          <Input
            name="title"
            value={note.title}
            onChange={handleChange}
            placeholder="value"
          />
        </FormControl>

        <FormControl mt={4}>
          <FormLabel>Content</FormLabel>
          <Textarea
            name="content"
            value={note.content}
            onChange={handleChange}
            placeholder="Content of the note"
          />
        </FormControl>

        <FormControl mt={4}>
          <FormLabel>Date: {note.date}</FormLabel>
          <Input
            name="date"
            value={note.date}
            onChange={handleChange}
            type="date"
          />
        </FormControl>
        <Button type="submit" colorScheme="purple" mt={3}>
          Save
        </Button>
      </form>
    </Box>
  )
}

export default EditNote

```

## Header.js

This is like the navigation bar of our application. We use [react-router](https://reactrouter.com/) to navigate between the components.

```jsx
// src/components/notes/Header.js

import React from 'react'
import { Flex, Button, Text } from '@chakra-ui/react'
import { FaEdit } from 'react-icons/fa'
import { Link } from 'react-router-dom'
import { ColorModeSwitcher } from '../../ColorModeSwitcher'

const Header = ({ setIsLogin }) => {
  const logOut = () => {
    console.log('Logged out')
  }

  return (
    <Flex justifyContent="space-between" w="100%" flexDirection="row">
      <Flex justifyContent="center" alignItems="center" mb={4}>
        <FaEdit size="24px" />
        <Text ml={4} textAlign="center" fontWeight="bold" fontSize="2xl">
          <Link to="/">Notes App</Link>
        </Text>
      </Flex>
      <Flex justifyContent="space-between">
        <Button variant="outline" size="md">
          <Link to="/create"> Add Note </Link>
        </Button>
        <Button onClick={logOut} mr={2} variant="solid" size="md">
          Logout
        </Button>
        <ColorModeSwitcher />
      </Flex>
    </Flex>
  )
}

export default Header

```

- `Link` in react router is like `a` tag in HTML.
- We have `Add Note` button for navigating to `/create` to `createNotes`
- We have `Logout` button for logging out of the account anytime.
- We have `ColorModeSwitcher` component, for adding the functionality of switching between light mode and dark mode.
- Also we should display the `Header` component only when a user is logged in.

## Home.js

Let's create a component for displaying notes. We get all the notes as an `Array` from the database. We use [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) to create a new array of note components and render it to the browser. 

```jsx
// src/components/notes/Home.js

import React, { useState } from 'react'
import { IconButton } from '@chakra-ui/button'
import {
  Flex,
  Grid,
  Box,
  Heading,
  HStack,
  Stack,
  Text,
} from '@chakra-ui/layout'
import { format } from 'timeago.js'
import { FaTrashAlt } from 'react-icons/fa'
import EditNote from './notes/EditNote'

const Home = () => {
  const [notes, setNotes] = useState([])

  // for deleting notes
  const deleteNote = id => {
    console.log('Notes deleted')
  }

  return (
    <Flex w="100%" flexDirection="column" mb={8}>
      {!notes.length ? (
        <Flex
          h={['30vh', '50vh']}
          w="100%"
          justifyContent="center"
          alignItems="center"
        >
          <Text fontSize='3xl' opacity="0.2">
            No Notes Added
          </Text>
        </Flex>
      ) : (
        <Grid
          templateColumns={[
            'repeat(1, 1fr)',
            'repeat(2, 1fr)',
            'repeat(3, 1fr)',
          ]}
          gap={6}
          m="0 auto"
          w={['100%', '90%', '85%']}
        >
          {notes.map(note => (
            <Box
              w="100%"
              p={5}
              shadow="md"
              borderWidth="1px"
              flex="1"
              borderRadius="md"
              key={note._id}
            >
              <Heading size="md" isTruncated>
                {note.title}
              </Heading>
              <Text my={4} noOfLines={[3, 4, 5]}>
                {note.content}
              </Text>
              <Stack spacing={4}>
                <HStack justifyContent="space-between">
                  <Text color="purple.500">{note.name}</Text>
                  <Text>{format(note.date)}</Text>
                </HStack>
                <HStack justifyContent="space-between">
                  <EditNote noteId={note._id}>Edit</EditNote>
                  <IconButton
                    onClick={() => deleteNote(note._id)}
                    colorScheme="red"
                    variant="solid"
                    size="md"
                    icon={<FaTrashAlt />}
                  ></IconButton>
                </HStack>
              </Stack>
            </Box>
          ))}
        </Grid>
      )}
    </Flex>
  )
}

export default Home

```

The design of note will be like this:

![note]()

You cannot see the notes yet, if `notes` is an empty array, it will display message "No Notes Added".

## Notes.js

Now we should add all the components to `Notes` component to render them when a user is logged in. You should know that react-router can only work if it is inside `BrowserRouter`. You can define which component should be rendered for specific route using `Route` of react router.

```jsx
// src/components/Notes.js

import React from 'react'
import { Flex } from '@chakra-ui/layout'
import { BrowserRouter as Router, Route } from 'react-router-dom'

import Home from './notes/Home'
import CreateNote from './notes/CreateNote'
import EditNote from './notes/EditNote'
import Header from './notes/Header'

const Notes = ({ isLogin }) => {
  return (
    <Router>
      <Flex flexDirection="column" alignItems="center" w="100%">
        <Header isLogin={isLogin} />
        <Route path="/" component={Home} exact />
        <Route path="/create" component={CreateNote} exact />
        <Route path="/edit/:id" component={EditNote} exact />
      </Flex>
    </Router>
  )
}

export default Notes

```

Check if all the router are working or not. Find the progress of the project [here]().