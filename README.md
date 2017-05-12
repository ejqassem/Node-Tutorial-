# Node-Tutorial-
Basic walk-through of node.js javascript framework 

This application is designed to give the beginning javascript developer a basic understanding of how node.js works and some basic examples demonstrating its uses. This tutorial is being built as a personal reference, but if there any issues in my code please email: ejqassem@gmail.com or create an issue on this repo. 

# Getting started with Node.js(Express): 
### 1) With any node.js application, it is important to start off importing dependencies:

```javascript
var express = require('express');
var bodyParser = require('body-parser');
var path = require('path');
```

In this code, we are just declaring three dependencies(node modules) to be used later on in the application: express, body-parser, and path. All of which were downloaded via the command line using the following code: 
```bash
  npm install <module name>
```
### 2) From here we need to *actually* set up our express application. 
To do this: we call on the express method on the returned express object using the function invocation operators and set it equal to a variable called app(common nodeJS convention). Additionally, we are setting a variable to equal a port of our choosing(commonly 8080). Using all capital letters in the naming of PORT is just to allow for easier tracing of the PORT and helps prevent user typing errors. 
```javascript 
var app = express(); 
var PORT = 8080; 
```

### 3) Next, it is important to set up a data-handler for incoming data from the client(via POST, etc...)
To do this: we use the bodyParser module that we already required in the code above and use four methods on the bodyParser object
```javascript 
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.text());
app.use(bodyParser.json({ type: "application/vnd.api+json" }));
```

