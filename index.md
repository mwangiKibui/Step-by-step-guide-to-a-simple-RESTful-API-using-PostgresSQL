## Step by step guide to a simple RESTful API using PostgresSQL.

[REST](https://www.edureka.co/blog/what-is-rest-api/) stands for REpresentational State Transfer. It is a technology that allows to create a data object, send the state of that object to the in a sever and return the object's values. REST refers to transferring "representations". We use the "representation" of a resource (data) to transfer a resource state from a server into an application state on the client-side.

REST is a set of design criteria and not the physical structure (architecture) of the system. It doesn't depend on the mechanics of HTTP protocols.

On the other hand, API stands for application program interface. It is like a common language between two software programs. An API uses an agreed-upon data format to send requests and responses back and forth between programs. An API states the rules and procedures for the communication between two programs to happen. This helps to make a point of contact (an endpoint) between these programs. We can use this concept to create a RESTful API.

A RESTful API works almost as the web does. Typically, you make an API request to the server and get a response back via an HTTP protocol.

![](rest-http-protocol.png)

[***Image source***](https://clevertechie.com/guides/96/what-is-rest-api-restful-web-services)

This request made to a server uses HTTP methods such as;

- GET - retrieve data from the server.
- POST - a good example of a POST where you fill some data in an HTML form and submit it to sever. POST methods help to submit specific data to be processed by the server.

- PUT- allows sending an update request to sever. PUT method allows modifying specified data values.

- DELETE - this allows you to make a request and inform the server that you want to delete some specified data values.

You can use the returned data and mimic it to build an application. The application will help your users to interact with the values of the returned data.

RESTful API can be developed will almost every programming language. In this guide, you'll learn the REST concept by building a RESTful API using Node.js. We will use Express to manage the server's HTTP protocols. We will build an interactive API. For that reason, we need a way to store our data. This guide will use SQL (A relation database management system) to manage our data.

Some reasons [why RESTful APIs are popular](https://www.serviceobjects.com/resources/articles-whitepapers/why-rest-popular) includes

- They are stateless and cacheable.
- High-Performance due to its cacheable architecture.
- Uniform client-server architecture. This separates the client from the server hence scalable server components and resources.
- They have a uniform interface. Each HTTP method (URL) is unique. This makes it easier to identify and manipulate self-descriptive resources using representations.
- RESTful API enables web applications built on various programming languages to communicate with each other in different environments.

### Goal

We will use a hand on todo list app scenario to create our RESTful API. This app complies with `CRUD` operations such as ;

- CREATE - adding a new todo.
- READ - view the todo list items.
- UPDATE the todo list. To update the todo list, we will use a toggle to distinguish between done and undone todo. This will capture the aspect of UPDATE.
- DELETE a todo.

![](a-todo-list.jpg)

These CRUD operations go hand in hand with HTTP post methods.

![](crud-operations-http-methods.png)

[***Image source***](https://www.edureka.co/blog/what-is-rest-api/)

### Prerequisites

This guide assumes you have prior knowledge of the following key areas.

- Basic knowledge of Node.js, A JavaScript framework.

- Be familiar with SQL. We will use SQL queries to communicate with our database. Some prior knowledge on how to write these queries will be of great importance.

- Basic knowledge of how to use Express. You need to be familiar with Express, a node.js package. Be able to create routes and manage a simple server with Express. Here is a [guide](/engineering-education/express/) to help you get started using Express.

- Be familiar with PostgreSQL. Postgress is a relational database that uses SQL queries to interact with data stored in database tables. We will use the PostgreSQL database to store our todo data. If you're familiar with using Mysql workbench or PHPadmin apache server (wamp and xxamp), it will be considerably easier to get you started with PostgreSQL. They use a relational database management system, just like Postgress. This beginner [guide](/engineering-education/mysql-with-node-js/) will help you learn how to write and execute SQL queries within your node.js applications.

### Overview

#### The application packages

The following packages will help us put the todo app in place.

- Express - [Express](https://www.npmjs.com/package/express) will help us make The API endpoints that will communicate with the database server. This allows us to have access to the resources (data) we want. This data will be accessed based on the HTTP standard methods, i.e., GET, POST, UPDATE and DELETE.

- CORS - [CORS](https://www.npmjs.com/package/cors) stands for Cross-Origin Resource Sharing. Allows us to bypass security applied to an API. We said earlier that an API is a set procedure for two programs to communicate. This means the two (client and server) have a different origin, i.e., accessing resources from a different server to the server you are in. For a RESTful API, the client and server have different origins. In this case, trying to make a request to a resource on the server will fail. You are getting something from that server, and it's not the server you (the client) are coming from. That is a security concern for the browser. This is a great concern for a RESTful API. Because they are meant to be consumed by other clients and servers. CORS comes into play to disable this mechanism and allow access to these resources. CORS will add a response header [access-control-allow-origins] and specify which origins are to be permitted the access. Cons help us to make sure that we are sending the right headers. This is done by sending the headers from the server to the client to inform the browser that the client can access the resources requested. Check this [guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/COR) to learn more about CORS.

- EJS - [EJS](https://www.npmjs.com/package/ejs) stands for embedded JavaScript. It is a template engine language that lets you generate HTML mark-up with plain JavaScript. Instead of serving static content, we can serve more dynamic content using EJS. EJS template is rendered on the server-side to produce an HTML document that the client can then receive. We will use the EJS template to create a client-side page for our RESTful API.

![](ejs-views.jpg)

- [PG](https://www.npmjs.com/package/pg) - REST architecture is based on resources. A resource is data that we want to perform operations on. The data can be stored in databases in tables, JSON, or any other form. In this case, we will use Postgres SQL (an SQL relation database) to store our data. SQL databases have unique identifiers that will help us identify which data (a record) we want to perform an operation(s). For example, the id of a todo. Pg makes it possible for Node.js to connect and communicate with PostgreSQL databases.

- [Nodemon](https://www.npmjs.com/package/nodemon) - this a `dev` package (not needed for the app to function). Nodemon ensures that the server is running whenever you make changes. When you make save changes, you don't have to re-run the server. Nodemon will handle this for you. It saves you a couple of keystrokes in your Node.js server development pipeline.

#### The application structure

This is how we will lay down our todo app.

![](project-structure.png)

### Setting up the project

Make sure you have [Node.js](https://nodejs.org/en/download/) runtime installed on your computer. Upon running `node –v`, you will get the Node.js version `v14.16.0` installed on your computer, which checks that Node.js is successfully installed.

In your desired folder, run the following to initialize your Node.js project.

```bash
npm init
```

Answer the relevant questions, and then follow through to the next steps.

Alternatively, you can run `npm init -y' to auto initialize your project with NPM default values. Check this [guide](/engineering-education/beginner-guide-to-npm/) to understand how to use NPM.

### Install the necessary dependencies

Install all the Node.js Packages we discussed above as follows

```bash
npm install cors ejs express pg
```

and

```bash
npm install --save-dev nodemon
```

### Setting up the PostgreSQL database

Install the following PostgreSQL environments.

- [PostgreSQL](https://www.postgresql.org/download/)an opensource relational databse management system.
- [pgAdmin](https://www.pgadmin.org/download/), standalone destop application for managing PostgreSQL databases.

Once installed and well configured, create a database ad a table to work with.

- Create a database, `My_todos_db`.

```SQL
CREATE DATABASE test
```

- Create a table, `todos`.

```SQL
CREATE TABLE todos (
  id SERIAL PRIMARY KEY,
  title VARCHAR(100) NOT NULL,
  checked  Boolean NOT NULL)
```

In the `src` folder, create a `config` folder and then a `db.js` file. We will configure the database in the `db.js` file as follows:

```js
const Pool = require("pg").Pool;
const pool = new Pool({
    user:'postgres',
    host:'localhost',
    database:'name_of_your_database', // My_todos_db
    password:'your_password', //added during PostgreSQL and pgAdmin installation
    port:'5432' //default port
});

module.exports = pool;
```

### Setting up the server

In the `src` folder, create an `index.js` file, and configure the application server as follows:

```js
const express = require("express");
const cors = require("cors");
const app = express();

app.use(express.json());
app.use(cors());

app.get("/", (req, res) =>{
    res.send("hello world!");
});

const PORT = process.env.PORT || 4000;

app.listen(PORT, () => {
    console.log(`app started on port ${PORT}`)
});
```

#### Test if the server is working

To start the server, configure the `scripts` object in `package.json` as follows.

```bash
"dev": "nodemon ./src/index.js"
```

Run to `npm run dev` start the server.

![](start-the-server.jpg)

Open `http://localhost/4000` In a browser. This should give you a response `hello world!`.

The server is up and running And we can do away with `app.get("/", (req, res) =>{res.send("hello world!");`

Any changes that you add to the server application will be restarted by Nodemon, no need to re-run the server again.

![](nodemon-restart-the-server.jpg)

### Setting up the routes

Create a `routes` folder, in it create a `todos.js` file. Here we will configure our routes as follows:

```js
const express = require("express");
const router = express.Router();

//Get all todos.
router.get('/', async (req,res) => {
});

//Create a todo.
router.post('/todo', async (req,res) => {
});

//Update a todo.
router.put('/todos/:todoId', async (req,res) => {
});

//Delete a todo.
router.delete('/todos/:todoId', async (req,res) => {
});

module.exports = router;
```

For the routes to work out, we need to configure them in the `index.js` file. To do this, we add the following changes to the `src/index.js` file:

```js
//import the routes
const todoRoutes = require('./routes/todos');

//configure the app.
app.use(todoRoutes);
```

### Setting up the controllers

The controllers are responsible for handling the functionality exposed by the routes. To set the controllers, create a folder `controllers` and create a file `Todo.js`. In this file, we will add all our needed SQL queries such as SELECT, INSERT, UPDATE AND DELETE functionalities as follows:

```js
const db = require("../config/db");

class Todo {
  //get all todos.
  async getTodos() {
    let results = await db.query(`SELECT * FROM todos`).catch(console.log);
    return results.rows;
  }

  //create a todo.
  async createTodo(todo) {
    await db
      .query("INSERT INTO todos (title, checked) VALUES ($1, $2)", [todo.title,false,])
      .catch(console.log);
    return;
  }

  //update a todo.
  async updateTodo(todoId) {
    //get the previous todo.
    let original_todo = await db
      .query(`SELECT * FROM todos WHERE id=$1`, [parseInt(todoId)])
      .catch(console.log);
    let new_checked_value = !original_todo.rows[0].checked;

    //update the checked todo
    await db
      .query(`UPDATE todos SET checked=$1 WHERE id=$2`, [new_checked_value,parseInt(todoId),])
      .catch(console.log);
    return;
  }

  //delete a todo.
  async deleteTodo(todoId) {
    await db.query(`DELETE FROM todos WHERE id=$1`, [parseInt(todoId)]).catch(console.log);
    return;
  }
}

module.exports = Todo;
```

### Linking the controllers to the routes

For the routes to really function, we need to link them with their respective controllers. In the `todos.js` file in the `routes` folder, add the following changes:

```js
//import the controller
const Todo = require('../controllers/Todo');

//Get all todos.
router.get('/', async (req,res) => {
    let todos = await new Todo().getTodos();
});

//Create a todo.
router.post('/todo', async (req,res) => {
    let {title} = req.body;
    await new Todo().createTodo({title},res);
});

//Update a todo.
router.put('/todos/:todoId', async (req,res) => {
    let {todoId} = req.params;
    await new Todo().updateTodo(todoId,res);
    let todos = await new Todo().getTodos();
});

//Delete a todo.
router.delete('/todos/:todoId', async (req,res) => {
    let {todoId} = req.params;
    await new Todo().deleteTodo(todoId);
    let todos = await new Todo().getTodos();
});
```

### Setting up the views

We will set the EJS views that will be rendered to the client-side. EJS views work the same as HTML elements such as buttons and forms. Views will help us trigger the necessary actions such as adding, deleting, or updating a todo.

Set up the CSS and views folders, as shown in the application structure`.

![](setting-the-ejs-views.jpg)

![](css-styling.jpg)

We'll include the following views.

1. A home page (`home.ejs`) to include any EJS template that we add to our todo app.

```ejs
<%- include('../partials/header.ejs') %>
    <section class="home-page">
        <div class="container">
            <div class="row">
                <div class="col-12 col-md-12 col-sm-12">
                    <div class="todo-content">
                        <h4 class="todo-heading">My todos.</h4>
                        <%- include('../partials/todos.ejs') %>
                        <%- include('../partials/add-todo.ejs') %>
                    </div>
                </div>
            </div>
        </div>
    </section>
```

2. A header (`header.ejs`)- this will include the following;

- A todo herder.

- Link the `custom.css`, `bootstrap.min.css` and, `main.js` We add the update and delete functionalities linking to the views in this main.js` as shown below;

```js
//updating a todo.
function updateTodo(todoId) {
    //contact server
    return $.ajax({
        method: "put",
        url: `/todos/${todoId}`,
        contentType: "application/json",
        cache: false,
        error: (error) => {
            console.error(error);
        },
    });
}

//deleting a todo.
function deleteTodo(todoId) {
    //contact server
    return $.ajax({
        method: "delete",
        url: `/todos/${todoId}`,
        contentType: "application/json",
        cache: false,
        success: () => {
            location.reload();
        },
        error: (error) => {
            console.error(error);
        },
    });
}
```

This how the `header.ejs` should look like, after adding a header, the CSS files, and `main.js`.

```ejs
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todolist app</title>
    <link href="/static/css/bootstrap.min.css" rel="stylesheet" />
    <link href="/static/css/custom.css" rel="stylesheet" />
    <script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
    <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
    <script src="/static/js/main.js"></script>
</head>

<body>
```

3. Add new todo (`add-todo.mejs`) - a POST formethod for adding new todos.

```ejs
<div class="add-todo">
    <h5 class="add-todo-heading">Add a todo.</h5>
    <form class="add-todo-form" method="POST" action="/todo">
        <div class="form-group form-title">
            <label for="title"> Title </label>
            <input id="title" type="text" class="form-control" name="title" placeholder="What do you want to do?" />
        </div>
        <div class="form-group form-submit">
            <button type="submit" class="btn btn-primary">add todo</button>
        </div>
    </form>
</div>
```

4. Todo list (`todos.ejs`). a GET form method for adding new todos This will list down any todo we added to our todo list database. Every todo will have a delete button and a toggle to check a completed todo.

```ejs
<ul class="list-group">
    <% for(let i=0; i < todos.length; i++) { %>
        <li class="list-group-item d-flex justify-content-between align-items-center">
            <%= todos[i].title %>
            <div class="list-group-item-actions">
                <div class="form-check">
                    <input type="checkbox" class="form-check-input" onclick="updateTodo(<%=todos[i].id %>)" <%=todos[i].checked ? "checked" : "" %>/>
                </div>
                <button class="delete-todo-form-btn" onclick="deleteTodo(<%= todos[i].id %>)">
                    <i class="far fa-trash-alt"></i>
                </button>
            </div>
        </li>
        <% } %>
</ul>
```

Some CSS to style the views.

```css
.home-page {
    width:100%;
    height: 100;
    margin-top: 10px;
    padding:20px;
}

.todo-content {
    width:100%;
    
}

.todo-heading {
    width: 100%;
    text-align: center;
    margin: 10px 0px;
}

.list-group-item-actions{
    display: flex;
    justify-content: space-between;
}

.update-todo-form {
    margin-right: 5px;
}

.add-todo{
    width:80%;
    margin: 20px auto;
    padding: 10px 0px;
}

.add-todo-heading {
    width: 100%;
    text-align: center;
    margin: 10px 0px;
}

.add-todo-form{
    width:50%;
    margin:0px auto;
    display: flex;
    justify-content: space-between;
}

.form-title {
    width:80%;
    margin-right: 10px;
}

.form-submit {
    margin-top: 24px;
}

.delete-todo-form-btn{
    border: none;
    background: transparent;
    cursor: pointer;
}
```

>Check this project on Github and grab the bootstrap used to style the bootstrap elements such as buttons. Bootstrap CSS is specified on the `bootstrap.min.css` file.

To integrate the app with these views, we need to add these lines of code to our server (`index.js`).

```js
app.use(express.urlencoded({extended:true}));
app.set("view engine","ejs");
app.set("views","src/views/pages");
app.use('/static',express.static(`${__dirname}/public`));

```

This will;

- Serve the EJS engine templates.
- Serve static files such as `.css` files.

### Linking the views to the routes

To view all our functionalities to the backend client, we need to link the routes to the views.

To do this, add the following changes to the `todos.js` file in the `routes` folder.

```js
//Get all todos.
router.get('/', async (req,res) => {
    let todos = await new Todo().getTodos();
    return res.render('home',{todos});
});

//Create a todo.
router.post('/todo', async (req,res) => {
    let {title} = req.body;
    await new Todo().createTodo({title},res);
    return res.redirect('/')
});

//Update a todo.
router.put('/todos/:todoId', async (req,res) => {
    let {todoId} = req.params;
    await new Todo().updateTodo(todoId,res);
    let todos = await new Todo().getTodos();
    return res.render('home',{todos});
});

//Delete a todo.
router.delete('/todos/:todoId', async (req,res) => {
    let {todoId} = req.params;
    await new Todo().deleteTodo(todoId);
    let todos = await new Todo().getTodos();
    return res.render('home',{todos});
});
```

### Testing the application

To run the application:

```bash
npm run dev
```

Open in a browser, `http://localhost/4000` and interact with the application.