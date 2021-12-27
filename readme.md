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
---------------------------------------------------------
## Connecting Node to a Postgres Database Exercise Solution
Exercise Tasks

Here were the tasks involved in completing this exercise
### Task List
- Install the dotenv library
- Create environment variables from the information given in the exercise instructions
- Create the database connection file and make it consume the environment variables
- Familiarize yourself with the application files and folders
### Solution
- Installing Dotenv

If you struggled to get the dotenv package installed, there are written instructions in the lesson content for you to follow, as well as a link to the package documentation.

- Environment Variables

After installing Dotenv you have a .env file. The exercise instructions gave you some bits of information for connecting to the database, your challenge was to turn them into environment variables and use them to create the database connection.

Below is an example of a working dotenv file, your variables names do not have to match these, but the variable names should be indicative of what they hold.
```sh
POSTGRES_HOST=localhost
POSTGRES_DB=full_stack_dev
POSTGRES_USER=full_stack_user
POSTGRES_PASSWORD=password123
```
- Database connection file

Here is an example database file.
```sh
import dotenv from 'dotenv'
import { Pool } from 'pg'

dotenv.config()
const {
    POSTGRES_HOST,
    POSTGRES_DB,
    POSTGRES_USER,
    POSTGRES_PASSWORD,
} = process.env 

const client = new Pool({
    host: POSTGRES_HOST,
    database: POSTGRES_DB,
    user: POSTGRES_USER,
    password: POSTGRES_PASSWORD,
})

export default client
```
### Things to note here:
- The dotenv.config() line initializes the environment variables. You can't access the env vars unless this line exists in your code, it typically goes as close to the beginning of the program as possible.
- You don't have to destructure the variables out of process.env like I did here, this was just to show you that process.env is just a regular old Javascript object that can be destructured, and it cleans things up a little.
- Remember that as far as we're concerned in this course, pool is just a connection to the database.

----------------------------------------
## Migrations
Before we get started, I feel its important to say that database migrations could be a big topic, with much more time devoted to it than I am going to spend here. That being said, the complexity level of migrations can easily go beyond what we need for the application we are building in this course, so I have tried my best to contain the topic of migrations to just what is needed to implement this kind of project well, without losing any of the most important concepts. There will be resources available at the end of this lesson to help you go further with migrations, and I hope you find this section to be a helpful start!That out of the way, its on with the show. In this video, I build a case for why migrations are helpful and introduce the foundational concepts of how they work.
### Video Summary
- Why we need migrations
- Migrations are a record of a change made to the schema of a database, with documented instructions to implement and rollback that change

### Documentation
- This is the npm library we will use for database migrations: [DB-migrate](https://github.com/db-migrate/node-db-migrate)
## Migrations Example between Coworkers
## Instructions to install db-migrate
- Install the global package `npm install -g db-migrate`
- Install the package to the project `yarn add db-migrate db-migrate-pg`
- Add a database.json reference file in the root of the project. Later, when we are working with multiple databases - this will allow us to specify what database we want to run migrations on. Here is an example `database.json`, you will just need to change the database names:
```sh
{
  "dev": {
    "driver": "pg",
    "host": "127.0.0.1",
    "database": "fantasy_worlds",
    "user": "magical_user",
    "password": "password123"
  },
  "test": {
    "driver": "pg",
    "host": "127.0.0.1",
    "database": "fantasy_worlds_test",
    "user": "test_user",
    "password": "password123"
  }
}
```
- Create a migration `db-migrate create mythical-worlds-table --sql-file`
- Add the SQL you need to the up and down sql files
- Bring the migration up` db-migrate up`
- Bring the migration down `db-migrate down`

------------------------------------------------
## 7-Quiz: Migrations
### Question 1 of 3
Select the correct down migration to match this up migration CREATE TABLE cats(id SERIAL PRIMARY KEY, name VARCHAR, type VARCHAR, description text);
- DROP TABLE cats(id SERIAL PRIMARY KEY, name VARCHAR, type VARCHAR, description text);
- (✓) DROP TABLE cats;
- REMOVE TABLE cats;
- REMOVE TABLE cats(id SERIAL PRIMARY KEY, name VARCHAR, type VARCHAR, description text);
### Question 2 of 3
Complete the reasons that migrations are helpful
|Migrations|Reasons|
|---------|--------|
|Migrations are records of|changes made to the database schema|
|Migrations contain instructions for| how to enact and rollback a specific change to the database|
|Using migrations to manage database schema changes in a project makes it easier to|keep databases across various environments synced together.|
### Question 3 of 3
Select the correct down migration to match this up migration ALTER TABLE cats ADD COLUMN favorite_toy VARCHAR;
- (✓) ALTER TABLE cats DROP COLUMN favorite_toy;
- ALTER TABLE cats ADD COLUMN owner_id REFERENCES user(id);
- DROP TABLE cats;
- DELETE * FROM cats WHERE favorite_toy IS NULL;

------------------------------------------
## 8-EXERSICE MIGRATION
### Exercise Instructions

In this exercise you pick up where you left off with a Node app that contains only a database connection. You must do the following:
Exercise tasks
- Task List
    - (✓) Setup db-migrate
    - (✓) Create a migration to add a books table to the database. A single book entry looks like this: `book  {title: "Bridge to Terabithia", author: "Katherine Paterson", total_pages: 208, type: "childrens", summary: "A good book"}`
    - (✓) Run the migration
    - (✓) Verify that the table was created by 
the migration by going into psql and checking for the table

-----------------------------------------------

## Migrations Exercise
### Getting Started

This repo contains a basic Node and Express app to get you started in constructing an API.
### Workspace
This exercise can be done inside of this Udacity workspace. To ready your environment follow these steps:
### In a terminal tab, create and run the database:
- switch to the postgres user su postgres
- start psql psql postgres
- in psql run the following:
  - CREATE USER full_stack_user WITH PASSWORD 'password123';
  - CREATE DATABASE full_stack_dev;
  - \c full_stack_dev
  - GRANT ALL PRIVILEGE

----------------------------------------------
## My Solution
```sh
npm i dotenv
npm i db-migrate -g
npm i db-migrate db-migrate-pg -g
db-migrate create books-table --sql-file
psql -U postgres -h localhost -W

postgres=# \c full_stack_dev
full_stack_dev=# \dt
```
- drop down migration
```sh
DROP TABLE books;
```
- up migration
```sh
CREATE TABLE books (id SERIAL PRIMARY KEY, title VARCHAR(100),
 auther VARCHAR(100), total_pages INTEGER, type VARCHAR(50),
 summary TEXT);
```
----------------------------------------------------------------
## Migrations Exercise Solution & Review
Exercise tasks
### Task List
- Task List
    - (✓) Setup db-migrate
    - (✓) Create a migration to add a books table to the database. A single book entry looks like this: `book  {title: "Bridge to Terabithia", author: "Katherine Paterson", total_pages: 208, type: "childrens", summary: "A good book"}`
    - (✓) Run the migration
    - (✓) Verify that the table was created by 
### Solution
#### DB-migrate
Written instructions for installing db-migrate are in the lesson content.
#### Create the migration

With the library set up correctly, you should have been able to run the command `db-migrate create books-table --sql-file`, And have it generate the corresponding folder and files in the migrations folder of your app.
### Write the SQL
With the migration files created, you needed to go into the sqls folder, find the up file and fill it with this query:
```sh
CREATE TABLE books (
    id SERIAL PRIMARY  KEY,
    title VARCHAR(150),
    total_pages integer,
    author VARCHAR(255),
    type VARCHAR(100),
    summary text
);
```
You'll see that I added an id column even though it wasn't mentioned. This is because every table should have a primary key, and none of the other data points associated with book were suitable as a primary key because they can't be guaranteed to be unique. Remember that semicolon!

Note - What types did you choose for your columns? Most of these are pretty straightforward, you'll note I added author as a full 255 character string, just in case a book has multiple authors.

- Now for the down file!
```sh
DROP TABLE books;
```
- Run the migration

Running `db-migrate up` in the terminal should create the table books.

- Check the migration

And after running dt to display tables you should see a table books included in the list! (There may be other system tables already in the database, so you'll need to check for your table specifically. But if you see a message like "No relations exist" then something went wrong.)

---------------------------------------

## 10-Introduction to Models
In the previous lesson we spent a lot of time on tables and CRUD, but now our Node application is going to be triggering all of those actions for us. The focus of this video is performing CRUD operations on the database from within a Node program.
### Video Summary
- Walkthru for creating the file and folder structure for models in the Node app
- Walktrhu creating a model file with methods
- Tables hold a list of items that share properties (columns), models are a class in our code that can be used as a template to create items that are stored as rows in the table.
### Documentation
- Express [documentation](https://expressjs.com/en/guide/routing.html)

- create folder models to save classes deals with data and inside it create for every table amodel to work on its fields and rows for our example it will be something like:
```sh
import client from '../database'
export type Weapon{
  id: Number;
  name: string;
  type: string;
  weight: number;
}

export class MythicalWeaponStore {
  async index(): Promise<Weapon[]>{
    try{
      const conn = await client.connect()
      const sql = 'SELECT * FROM Mythical_weapon'
      const result = await conn.quary(sql)
      conn.release()
      return result.rows 
    }catch(e){
      throw new Error (`Cannot get weapon ${e}`);
    }
  }  
}
```

---------------------------------------------------------------------

## 11- Quiz: Models
### Quiz Question
Match the model method to its CRUD operation
|Model Method|CRUD_Operation|
|--------------------------|---------------------------|
| async create(p: Product): Promise
{ 
  const conn = await Client.connect()
  const sql = 'INSERT INTO products (name, price) VALUES($1, $2) RETURNING *' 
  const result = await conn.query(sql, [p.name, p.price] 
  const product = result.rows[0]
  conn.release() 
  return product
  }| CREATE |
| async delete(id: string): Promise
 {
  const conn = await Client.connect()
  const sql = 'DELETE FROM products WHERE id=($1)'
  const result = await conn.query(sql, [id])`
  } | DELETE |

--------------------------------------

## 12-Exersice Models
### Models Exercise Tasks
In this exercise you are tasked with building out a model with all CRUD actions for the books table you created in the last exercise
### Task List
- Create the models folder and a model file for the books table
- Create the Typescript type for the model
- Build out methods for all CRUD actions
### Helpful Reminder:
We are using the books table from the previous exercise, here was the table schema from that exercise for reference:
```sh
book( id SERIAL PRIMARY  KEY, title VARCHAR(150), total_pages integer, author VARCHAR(255),  type VARCHAR(100), summary text);
```
------------------------------------------
## StoreFront BackEnd Project README
## page 1 
### Getting Started

This repo contains a basic Node and Express app to get you started in constructing an API. To get started, clone this repo and run yarn in your terminal at the project root.
### Required Technologies
Your application must make use of the following libraries:
- Postgres for the database
- Node/Express for the application logic
- dotenv from npm for managing environment variables
- db-migrate from npm for migrations
- jsonwebtoken from npm for working with JWTs
- jasmine from npm for testing

### Environment
### Workspace
This project can be done inside of this Udacity workspace. To ready your environment follow these steps:
#### In a terminal tab, create and run the database:
- switch to the postgres user su postgres
- start psql psql postgres
- in psql run the following:
  - CREATE USER shopping_user WITH PASSWORD 'password123';
  - CREATE DATABASE shopping;
  - \c shopping
  - GRANT ALL PRIVILEGES ON DATABASE shopping TO shopping_user;
- to test that it is working run \dt and it should output "No relations found."

#### In the 2nd terminal:
Migrations to set up the database table for books from the last section are included in this exercise. To run them, follow the instructions below:
-  install yarn npm install yarn -g
-  install db-migrate on the machine for terminal commands npm install db-migrate -g
-  check node version node -v - it needs to be 10 or 12 level
-  IF node was not 10 or 12 level, run
-    npm install -g n
-    n 10.18.0
-    PATH="$PATH"
-    node -v to check that the version is 10 or 12
- install all project dependencies yarn
- to run the migrations ``
- to test that it is working, run yarn watch should show an app starting on 0.0.0.0:3000
#### Local Environment
If want to do this project on your local computer and you already have docker installed, there is a docker file provided for you with generic content. Note that you may need to update this file to fit your computer in order to use it locally.
Steps to Completion
-  Plan to Meet Requirements
-  DB Creation and Migrations
-  Models
-  Express Handlers
-  JWTs
-  QA and Readme
#### Go to the following pages to get started on the project!
# API Requirements
The company stakeholders want to create an online storefront to showcase their great product ideas. Users need to be able to browse an index of all products, see the specifics of a single product, and add products to an order that they can view in a cart page. You have been tasked with building the API that will support this application, and your coworker is building the frontend.

These are the notes from a meeting with the frontend developer that describe what endpoints the API needs to supply, as well as data shapes the frontend and backend have agreed meet the requirements of the application. 

## API Endpoints
#### Products
- Index 
- Show (args: product id)
- Create (args: Product)[token required]
- [OPTIONAL] Top 5 most popular products 
- [OPTIONAL] Products by category (args: product category)

#### Users
- Index [token required]
- Show (args: id)[token required]
- Create (args: User)[token required]

#### Orders
- Current Order by user (args: user id)[token required]
- [OPTIONAL] Completed Orders by user (args: user id)[token required]

## Data Shapes
#### Product
-  id
- name
- price
- [OPTIONAL] category

#### User
- id
- firstName
- lastName
- password

#### Orders
- id
- id of each product in the order
- quantity of each product in the order
- user_id
- status of order (active or complete)

--------------------------------------------------------------
## Models Exercise Solution
### Files and Folders

Create the models folder and books model file. The directory structure should look like this:src --> models --> book.ts

`Extra`: Did you notice or wonder why its the books (plural) table in the database, but the book (singular) file for the model? That's because the database table will hold many books, but the model file is `defining` what a book is for our application. The model is represented as a class, each book row in the database will be an instance of the book model. I've always felt its so cool how all of that fits together.
### Typescript Type
In book.ts one of the first things we need is to define the Typescript type for book. This will allow us to pass this type around and use it in our function return types. Here is what the type should look like:
```sh
export type Book = {
     id: number;
     title: string;
     author: string;
     totalPages: number;
     summary: string;
}
```
### CRUD methods
In the video we didn't build out all the CRUD model methods, but here you were tasked with building out all of them. Here is a copy of the entire model file:
```sh
// @ts-ignore
import Client from '../database'

export type Book = {
     id: number;
     title: string;
     author: string;
     totalPages: number;
     summary: string;
}

export class BookStore {
  async index(): Promise<Book[]> {
    try {
      // @ts-ignore
      const conn = await Client.connect()
      const sql = 'SELECT * FROM books'

      const result = await conn.query(sql)

      conn.release()

      return result.rows 
    } catch (err) {
      throw new Error(`Could not get books. Error: ${err}`)
    }
  }

  async show(id: string): Promise<Book> {
    try {
    const sql = 'SELECT * FROM books WHERE id=($1)'
    // @ts-ignore
    const conn = await Client.connect()

    const result = await conn.query(sql, [id])

    conn.release()

    return result.rows[0]
    } catch (err) {
        throw new Error(`Could not find book ${id}. Error: ${err}`)
    }
  }

  async create(b: Book): Promise<Book> {
      try {
    const sql = 'INSERT INTO books (title, author, total_pages, summary) VALUES($1, $2, $3, $4) RETURNING *'
    // @ts-ignore
    const conn = await Client.connect()

    const result = await conn
        .query(sql, [b.title, b.author, b.totalPages, b.summary])

    const book = result.rows[0]

    conn.release()

    return book
      } catch (err) {
          throw new Error(`Could not add new book ${title}. Error: ${err}`)
      }
  }

  async delete(id: string): Promise<Book> {
      try {
    const sql = 'DELETE FROM books WHERE id=($1)'
    // @ts-ignore
    const conn = await Client.connect()

    const result = await conn.query(sql, [id])

    const book = result.rows[0]

    conn.release()

    return book
      } catch (err) {
          throw new Error(`Could not delete book ${id}. Error: ${err}`)
      }
  }
}
```
