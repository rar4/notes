
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
Args: receiver_address, message.
Sends message to receiver address with content of message using smtp lib.
##### Make_URL
Args: page - title of sub-page of a site, args - python dict which stores URL argument names as keys and its values.
Constructs URL to given sub-page of a site with given arguments.
##### Error
Args: message
Renders an error with a given message
##### Login_required
Checks weather user that accessing decorated route is logged in, if not, redirects to log in page.
##### Enter
Args: name, passwd
If credentials are right, logs in user, if not returns none
##### Db_Exec
Args: query, args
Executes query in SQLite database with arguments from args and returns what database yields, handles SQLite integrity error


#### generation.py


Connects to Gemini through google cloud API, contains all prompts, setup and logic for connection. 

##### Generate_Idea
Args: query - topic on which ideas are generated, concise - boolean object that decides weather to generate and idea on a given topic or to make a description to a given title.

Constructs prompt, gives it to llm then turns llm response object to string and returns it.

#### fetch_image.py
Connects to pexels API 
##### Fetch_Image
Args: query
Searches for image with pexels API and returns link to it, if can't find any, returns link to image with message that image is not found.

#### brainstorm.db 
Contains all the necessary SQL tables for project.

schema:
CREATE TABLE User (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            username TEXT NOT NULL UNIQUE,
            password_hash TEXT NOT NULL);
CREATE TABLE sqlite_sequence(name,seq);
CREATE TABLE BrainstormSession (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            user_id INTEGER NOT NULL,
            topic TEXT NOT NULL,
            idea_description TEXT NOT NULL,
            idea TEXT NOT NULL,
            image_url TEXT NOT NULL,
            timestamp TEXT NOT NULL,
            FOREIGN KEY (user_id) REFERENCES User(id)
        );
#### pexest.txt
Contains pexels API key.

#### generation.txt
Contains title of google cloud project.

#### Templates

Folder that contains jinja templates for the flask application.

##### gate.html
Wrapper template for pre-login pages.
##### layout.html 
Wrapper for post login pages.
##### login.html
Login form page
##### register.html
Register form page
##### index.html
The main page with 
##### brainstorm.html
Brainstorming session page, displays idea title and image, activates endBrainstorm function via accept idea button and reloads page via next idea button.
##### error.html
Page, that contains error message and link to main page
##### history.html
Contains table of whole previous user ideas.
Columns: Topic, Idea, View Idea (link idea page).
##### idea.html
Contains image, idea, its description, and link to the main page.



#### Static
Contains static files
##### script.js
Contains javascript
###### endBrainstorm
Redirects to idea route with idea title and image link as arguments