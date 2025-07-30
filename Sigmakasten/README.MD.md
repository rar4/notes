
# Brainstorm
### Video Demo:  https://youtu.be/s9IY7oUBzT8?feature=shared
### Description:
My project is a flask web application, that generates ideas on a given by user topic with connection to the Gemini through google cloud API, pexels API for idea illustrating, SMTPlib for email authentication and password resetting.  Using SQLite databases for storing user and ideas data.

#### app.py

This file is a central part of my application, it contains routes made with flask and biggest part of business logic. 



**Now I will explain briefly each function:**

##### Register
GET: renders registration template
POST: sends authentication link with user data on email in order to confirm that user owns this email
#####  Confirm 
Adds user to db and logs him in.
##### Login
GET: Renders log in template
POST: Checks weather user with given login exists and weather entered password is right, if yes, logs user in, if no, returns error.
##### Reset
Generates and stores authentication code in flask session and renders reset password template.
##### Confirm_Reset
POST: Sends authentication link for changing password to user email.
GET: Changes user password in db and logs user in
##### Index
Renders main page
##### Brainstorm 
Generates idea on a topic given by user, renders this idea and image

