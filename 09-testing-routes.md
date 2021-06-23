# THE **MERN** STACK — TESTING ROUTES

In this chapter we are going to test each and every route and check it's working. We are going to use [Postman](https://www.postman.com/) for testing the application.

## Postman

It is a interactive application that would simplify the API testing process. Create an account in [postman](https://www.postman.com/) and open a workspace. You can easily send the request in a few simple steps. Make sure your application is running.

Let's send the first request to our applications root url.

![postman get request to the root url]()

1) Select the type of HTTP request you want to send.
2) Enter the URL in the box.
3) Hit **Send** button, you can see the response in `response` window.

You can send the data in POST request in the `body` tab.

## `/register`

Let's register a user. We should send the required data in the `body` tab of the request. You must get the success message as the response. Otherwise, it will send the error message.

![Register a user]

You can also find that the user get created in our database like this:

![first user in database]()

Let's try sending another request with the same email. This should return a response saying the email already exists.

![response saying email already exists]()

We can say that our `/users/register` route is working properly as expected.

## `/login`

Let's try to login. This should send the `token` and success message.

![login successful]()

Try logging in with wrong password. It should return an error messsage.

![wrong password error message]()

Try logging in with email that is not exist in your database. This should return the error message saying `User does not exist`.

![User does not exist]()

## `/verify`

Let's verify the token. You should add the token to the header `Authorization` of the request.

In the headers tab, add the `Authorization`  key with any value. You should get `false` if the token is not correct.

![incorrect token]()

Copy the token you get after logging in and try to add that token as `Authorization` value. You should get `true`.

![valid token]()

## `/api/notes`

## Creating notes

Try to create notes by sending POST request to `/api/notes`. Firstly you should see the error `Invalid Authentication`, meaning there is no token sent along the request. This is the way to aloow only logged in users can use our application.

![Invalid Authentication]()

If you send an invalid token, it returns the same error.

![invalid token]()

Now try to add the correct token just like we did before and send the request. You should get the success message.

![notes added]()

You can also find that the notes has been added to the database under `notes` collections.

![notes in database]()

## Reading notes

Send a GET request to the same end point with a valid token in the header. You should get all the notes you added in the response.

![notes]()

Copy the `_id` of the notes. Try to get the single notes using this id.

![single notes]()

## Update notes

Use the `id` and create a PUT request with new content in the request body. You shoud get the success message saying `Notes updated`.

![updated notes]()

You should also find the notes updated in the database.

## Delete notes

You cand send the DELETE request to the route using specific id of the notes you want to delete. You should get the success message and notes will be deleted from the database.

![deleted notes]()

If everything works fine, let's continue working on the front-end of our project. If any error occurs, try to troubleshoot the error using the message you get in the response. With this I believe your backend will be perfect and can respond correctly for the HTTP requests.