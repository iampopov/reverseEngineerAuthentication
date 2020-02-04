# Reverse Engineering Code

To reverse engineer the starter code provided and create a tutorial for the code.

```
AS A developer

I WANT a walk-through of the codebase

SO THAT I can use it as a starting point for a new project
```

## Business Context

When joining a new team, it is expected to inspect a lot of new code. Rather than having a team member explain every line for I will dissect the code by myself, saving any questions for a member of the team.

## Criteria

```md
GIVEN a Node.js application using Sequelize and Passport
WHEN I follow the walkthrough
THEN I understand the codebase
```

---

## The Passport.js tutorial

Login.html and login.js files: the HTML file is a page layout file while the js file is the front end jQuery logic - when the form is submitted it validates an email and password entered. Once done sends the login request to /api/login/ route.

Members.html and members.js files: similar to above the HTML file is a page layout file while the js file is the front end jQuery logic. For members, there is only one API route that figures out which member is logged in and presents the data accordingly.

Signup.html and signup.js files. Similar logic with presentation / logic. The front end jQ, in this case, validates upon signup that password and email are not blank, if parameters are entered correctly it runs to sign up user function that does an API post route call to signup on the back-end. If all is successful it redirects the page to the HTML members route.

Html routes: there are three HTML routes to /, /login and /members. / has two options one sign up if the user is not a member and member page if they are.
When calling /login and the user already has an account redirects them to /members page, otherwise, login page. When /members is called isAuthenticated middleware is used to make sure only signed up users can access this route.

isAuthenticated.js file basic logic if a user is logged in access continue with the request (above), otherwise redirects them to the login page.

API routes .post route takes in email and password and writes it to User db, then redirects to login. There are two .get routes one for log out and one for login. log out route redirects to the main page. The login route returns the user’s email and id.

User.js model set parameters to email and password e.g. either parameter cannot be null, the password must be unique and both parameters must be a string. It also checks if an unhashed password entered by the user can be compared to the hashed password stored in db and also automatically hashes the password using hooks.

Passport.js requires npm package passport. Configures log in procedures as a user is signed in with a password. When the user is trying to sign up it checks db if the email and password entered are valid if either is wrong it returns an error message otherwise returns the result of the function.

Config.js establishes connection to our db used in index.js.

Index.js file requires all necessary packages such as access to the filesystem, path, sequelize, and others and establishes the connection between our database and ORM in this case sequelize.

Server.js requires all necessary npm packages such as the passport as we described the configuration above, it also creates express app and configures the middleware needed for authentication, it also tracks sessions of user’s login status, requires the routes as described above in api and html routes paragraphs and lastly, it synchronizes database and logging a message to the user upon success.
