# THE **MERN** STACK — NOTES CRUD API

Let's create the API to implement the **CRUD** of notes.

## API endpoints for notes

|URL|HTTP method|functionality|
|----|----|----|
|/api/notes|GET|fetches all the notes in database|
|/api/notes|POST|creates new note|
|/api/notes/:id|GET|fetches the data of note with that specified id|
|/api/notes/:id|PUT|replaces the data of note with new data|
|/api/notes/:id|DELETE|removes the specific note from database|

## Notes Router

Create a new file named `notesRouter.js` in routers folders. Try to implement the above mentioned routes, with the names of controller functions.

Here's the `notesRouter.js` file looks like.

```js
const router = require('express').Router()
const notesCtrl = require('../controllers/notes.js')
const auth = require('../middleware/auth.js')

// 1) Create and Read notes
router.route('/').get(auth, getNotes).post(auth, createNotes)

// 2) Read, Update and Delete note
router
  .route('/:id')
  .get(auth, getNote)
  .put(auth, updateNote)
  .delete(auth, deleteNote)

module.exports = router
```

Here, the `auth` middleware is also added to the method, so that it adds restricts only logged in users can access these routes.

Add the base route for this router in `index.js` file.

```js
// index.js

// ...
const notesRouter = require('./routes/notesRouter.js')
// ...

app.use('/api/notes', notesRouter)

// ...
```

## Notes Controller

Add a new file named `notesCtrl.js` to the `controllers` folder.

### Get Notes

The HTTP GET method corresponds to the Read operation in database. It returns all the data in the database.

```js
const Notes = require('../models/note.model.js')

const getNotes = async (req, res) => {
  try {
    const notes = await Notes.find({ user_id: req.user.id })
    res.json(notes)
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}
```

This method fetches all the notes created by the user who is currently logged in.

```js
// route '/:id'
// ...

const getNote = async (req, res) => {
  try {
    const note = await Notes.findById(req.params.id)
    res.json(note)
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}
```

The `:id` in the route is the paramater, it can be accessed from the `params` field of the request. Here, we fetch the note that corresponds to the id in the URL.

### Post Notes

The HTTP POST method corresponds to the Create operation in database. It creates a new entry in the database using the data in the request.

1) We create the note using the `Notes` model using the request data.
2) Save the note in the database and send the response `Notes created`.

```js
// ...

const createNotes = async (req, res) => {
  try {
    // 1. Create a note
    const { title, content, date } = req.body
    const newNote = new Notes({
      title,
      content,
      date,
      user_id: req.user.id,
      name: req.user.name,
    })

    // 2. Save note to database
    await newNote.save()
    res.status(200).json({ msg: 'Notes created.', type: 'success' })
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}
```

### Put Note

The HTTP PUT method corresponds to the Update operation of database. You can access the specific note using the `findOneAndUpdate` of `Notes` model.

```js
// ...

const updateNote = async (req, res) => {
  try {
    const { title, content, date } = req.body
    await Notes.findOneAndUpdate(
      { _id: req.params.id },
      {
        title,
        content,
        date,
      }
    )

    res.json({ msg: 'Notes updated', type: 'info' })
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}
```

### Delete Note

You can delete the note by id using the `findByIdAndDelete` method of Notes model.

```js
// ...

const deleteNote = async (req, res) => {
  try {
    await Notes.findByIdAndDelete(req.params.id)
    res.json({ msg: 'Note Deleted', type: 'success' })
  } catch (error) {
    return res.status(500).json({ msg: error.message })
  }
}

module.exports = { getNotes, createNotes, getNote, updateNote, deleteNote }
```

This completes the backend of our application. We implemented the authentication using JSON Web tokens and created an API for implementing **CRUD** operations.

You can find the progress of the project [here]().

In the next chapter, Let's test whether our pplication works as expected or not.