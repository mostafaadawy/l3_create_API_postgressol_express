## What we'll do

This lesson is all about the database facing side of our API. We'll cover the following topics
- What it means to connect to a database
- The role of environment variables
- Database migrations
- Models in NodeJS
- Testing models

## Helpful Preparation
-    If you need a refresher on Express, check out the Mozilla [docs](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/Introduction).
-    It will be greatly beneficial, though not strictly necessary, to understand MVC! This [article](https://betterexplained.com/articles/intermediate-rails-understanding-models-views-and-controllers/) is written for Ruby on Rails, but its a good breakdown of what MVC means and how it fits into the flow of requests and responses.

--------------------------------------------------
## A Tour of Our Node Application
### Video Summary
- Walk through of the starter Node app.
### Documentation
Here are links to the documentation for the libraries I talked about in the video
- [NPM Postgre](https://github.com/brianc/node-postgres)
- [Typescript watch](https://github.com/gilamran/tsc-watch)

## Where Databases Run
Since we are talking about connecting and running databases, I want to touch on where these databases are, because that can be confusing and there are lots of options.
- You can run a database on your computer and connect to it locally kind of like localhost.
- You can run a database on your own computer in a container system like Docker.
- Container systems are also common remotely, for instance in the Udacity workspace, you are connecting to a database running in a virtual machine.
- Services like AWS and Azure provide databases in the cloud.

In each of the situations above, the process of connecting to a database will look a little different, but you will still be connecting to a database as a user. Generally speaking, the more technologies or layers between you (or your application) and the database you want to use, the more complex it will be to connect to that database. To keep things simple for this course, the Node application and Postgres database will run in the same VM.
## Environment Variables
Working with sensitive information can be hard, especially when your application relies on keys and passwords in order to connect to and access databases or APIs. The instructions below will walk you through adding a library for environment variables in Node so that we can safely store information away from public eyes without moving it out of reach.
- The library we will use for environment variables is called dotenv. You can add it via npm or yarn like this: `yarn add dotenv`
- Once we have dotenv listed in the package.json dependencies, we need to create one file. Make a new file called `.env` in the root of the project. In that file, add this: `TEST_VAR=testing123`. This is our first environment variable!
- One last, super important step. The `.env` file hides sensitive information and makes it available to our application via a variable, so it holds a lot of really important, secret information. Information we don't want shared even in a respository. If a gitignore file exists in your project add the `.env` file there. If there isn't a gitignore, add a file called `.gitignore` to the root of the project and add a single line in the file that says just `.env`. If you include your env file in a public repository, you have completely negated the purpose of adding environment variables.
- The dotenv library documentation lives [here](https://github.com/motdotla/dotenv).

Note: your app should be agnostic to any environment so it can run on a local computer with any operating systems, docker, or the cloud. Other than storing the secret credentials, you can also use the `.env` file to set up portable environment variables, such as assigning a PORT number and determining development/test/production environment. To set up environment variables using dotenv library, [this blog post](https://medium.com/the-node-js-collection/making-your-node-js-work-everywhere-with-environment-variables-2da8cdf6e786) provides some tips and tricks to store, load, and organize NodeJS environment variables.
## Connecting the database
When we write the command `psql <username>` we enter the interactive terminal for working with Postgres. But with a backend application like the Node API we are creating in this course, we won’t be the ones using psql and connecting to a database. The only one making changes to the database will be our Node app. The Node application is going to run the same psql command as we do to connect to the database, but we need to add some additional information.
### Video Summary
- Adding the dotenv library walkthru
- Creating a file for the database connection

------------------------------------------------------------
## Quiz: Connecting Node to a Postgres Database
### Question 1 of 3
Which of the following are information that might be required to connect to a database?
- Your ip address
- (✓) Username
- Developer name
- Application name
- (✓) Password
- (✓) Database name
- (✓) Database location
### Question 2 of 3
What are the main rules of environment variables?
- (✓) Always ignore the env file in your gitignore
- Never ignore the env file in your gitignore
- (✓) Passwords should ALWAYS be found in the env file, never hard coded in a file
- Passwords should NEVER be found in the env file, always hard coded in a file
### Question 3 of 3
Which correctly accesses an environment variable? Imagine I have the line: TEST_VAR="test" in my .env file
- process.TEST_VAR
- process.TEST_VAR
- (✓) process.env.TEST_VAR
- (✓) const { TEST_VAR } = process.env
------------------------------------------
## Exercise: Connecting Node to a Postgres Database

Your turn! In this exercise you are given the same starter Node app we began with at the start of the lesson. This exercise is mostly to get you introduced to the Node application and database setup we will be using for the rest of the course exercises.
```sh
Database: full_stack_dev
Username: full_stack_user
password: password123
```
The `database` is running on port `3001` in this environment.

With this information, you should be able to create the database file in the Node app that will connect to the database. Remember to use environment variables - you'll have to install that library yourself!

`Note` The Udacity workspace system may treat the `.env` file as hidden. You can always find it in `terminal with ls -a`.
### Local Environment
If you want to do this exercise on your local computer and already have docker installed, there is a docker compose file provided for you with generic content in this [repo](https://github.com/udacity/nd0067-c2-creating-an-api-with-postgresql-and-express-project-starter). Note that you may need to update this docker file or setup to make it work for your machine.
## Exercise Tasks
Here are the tasks involved in completing this exercise
Task List

------------------------------------------------------------------

## My Solution
```sh
$ psql -U postgres -h localhost -W
Password: 
psql (13.5)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

postgres=# CREATE USER full_stack_user
postgres-# WITH PASSWORD 'password123';
CREATE ROLE
postgres=# CREATE DATABASE full_stack_dev;
CREATE DATABASE
postgres=# \c full_stack_dev
Password:
FATAL:  password authentication failed for user "postgres"
Previous connection kept
postgres=# \c full_stack_dev
Password:
You are now connected to database "full_stack_dev" as user "postgres".
full_stack_dev=# GRANT ALL PRIVILEGES ON DATABASE full_stack_dev TO full_stack_user;
GRANT
full_stack_dev=# \dt
Did not find any relations.
full_stack_dev=#
```
## On the other hand for installing server side and configuring it
- to make the scripts work we have to `setup ts-node`.
- and to change the configuration of tsconfig.json.
- Change the URL from `0.0.0.0:3000` modified to `localhost:3000`
- run `npm i`
- create `.env` file write the next statements to it
```sh
Database: full_stack_dev
Username: full_stack_user
password: password123
Port: 3000
```