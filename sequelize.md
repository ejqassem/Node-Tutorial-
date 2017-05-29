# Node-Tutorial-
Introducing the use of the sequelize NPM package 

This application is designed to give the beginning javascript developer a basic understanding of how node.js works, in conjunction with express and other node modules, and some basic examples demonstrating its uses. This tutorial is being built as a personal reference, but if there any issues in my code please email: ejqassem@gmail.com or create an issue on this repo. 

# Getting started with Sequelize: 
### 1) Before getting started with sequelize, it is important to get familiar with the mysql NPM package 
  - This will allow you to understand what the sequelize package is abstracting 

### 2) I am going to start off showing what a typical server.js file looks like using sequelize. 
  - It is important to understand the core files in your application on a high-level before diving into the details of utilizing mySQL databases
  
The following code was taken from my sequelize-burger repository(https://github.com/ejqassem/Sequelize-Burger-): 
```javascript 
// *****************************************************************************
// Server.js - This file is the initial starting point for the Node/Express server.
//
// ******************************************************************************
// *** Dependencies
// =============================================================
var express = require("express");
var bodyParser = require("body-parser");

// Sets up the Express App
// =============================================================
var app = express();
var PORT = process.env.PORT || 8080;

// Requiring our models for syncing
var db = require("./models");

// Sets up the Express app to handle data parsing
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.text());
app.use(bodyParser.json({type:"application/vnd.api+json"}));

// Static directory
app.use(express.static("./public"));

// Routes =============================================================
require("./routes/htmlRoutes.js")(app);
require("./routes/apiRoutes.js")(app);

// Syncing our sequelize models and then starting our express app
db.sequelize.sync().then(function() {
  app.listen(PORT, function() {
    console.log("App listening on PORT " + PORT);
  });
});
```
# Most of the above code should look familiar, but there are a few lines that should stick out: 
1) Requiring your entire models folder in a variable called "db". 
   - In a typical mysql app, a typical flow would be the initilization of the model/table in a connection.js file. Then, the creation of an orm.js file mapping the base functionality of the model(referencing the connection.js file). The orm.js file is further abstracted in the models.js file and even more so in the controller.js file. 
      - A simplified view of this looks like: connection -> orm(object relational mapping) -> models -> controller. 
2) Sequelize makes this process much easier. All of the above abstraction is taken care of in three files, two of which are already initialized for you via sequelize-cli of which I will go into later. 
3) Another section of code that should stick and probably won't make too much sense at this point is the db.sequelize.sync()... code 
    - At this point in the tutorial, the most important thing to take away from this code is that you have to sync your model/table after you initalize it. 
        - Forgetting to sync your database is synomyous to setting up your server.js file, but forgetting to set your server to listen to a particular port. 
