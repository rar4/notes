
# Brainstorm
### Video Demo:  https://youtu.be/s9IY7oUBzT8?feature=shared
### Description:
My project is a flask web application, that generates ideas on a given by user topic with connection to the Gemini through google cloud API, pexels API for idea illustrating, SMTPlib for email authentication and password resetting.  Using SQLite databases for storing user and ideas data.

#### app.py

This file is a central part of my application, it contains routes made with flask and biggest part of business logic. 

**Now I will explain briefly each function:**

##### Register
GET: renders registration page
POST: sends authentication link with user data on email in order to confirm that user owns this email
#####  Confirm 
Adds user to db and logs him in.
##### Login
GET: Renders log in page
POST: Checks weather user with given login exists and weather entered password is right, if yes, logs user in, if no, returns error.
##### Reset
Generates and stores authentication code in flask session and renders reset password page.
##### Confirm_Reset
POST: Sends authentication link for changing password to user email.
GET: Changes user password in db and logs user in.
##### Index
Renders main page.
##### Brainstorm 
Generates text with Gemini API and fetches image from pexels API on a topic given by user, converts text from markdown syntax to html syntax and renders text and image in brainstorm page, implements functionality which prevents llm from yielding same ideas in one session.
##### Idea
Generates description to previously generated idea and renders Idea, description and image in idea page.
##### History
Fetches all Ideas from db, that user have generated from newest to latest and renders them in history page.
##### Show_Idea 
Fetches image, title and description of Idea from db of a given id and displays it in idea page.
##### Logout
Clears all the flask session and redirects to login page.

#### utils.py

File that contains repetitive code which solves problems which are not connected to host communication. 

##### Crypt_Data
Args: data - string for encryption/decryption, encrypt - bool value which decides encrypt string or decrypt it.

Uses Fernet to encrypt or decrypt strings using secret key.
##### Send_Confirmation_Email
Args: receiver_address, message
Sends message to receiver address with content of message using smtp lib.
##### Make_URL
Args: page 

