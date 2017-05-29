# Node-Tutorial-
Basic walk-through of node.js javascript framework 

This application is designed to give the beginning javascript developer a basic understanding of how node.js works, in conjunction with express and other node modules, and some basic examples demonstrating its uses. This tutorial is being built as a personal reference, but if there any issues in my code please email: ejqassem@gmail.com or create an issue on this repo. 

# Getting started with Node.js(Express.js): 
### 1) With any node.js application, it is important to start off importing dependencies:

```javascript
var express = require('express'); //import express module 
var bodyParser = require('body-parser'); //import bodyParser module 
var path = require('path'); //import path module 
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
### 4) Let's initialize some data to manipulate in later code: 
```javascript 
var characters = [{
  routeName : "goku"
  name: "Goku",
  powerLevel: 9001, 
  age: 40
}, 
{
  routeName: "vegeta", 
  name: "Vegeta", 
  powerLevel: 9000,
  age: 42
}];
```

### 5) Routing: 
Routing is the meat-and-potatoes of the express.js node module. Basically, developers define application end-points in the server-side code and how they respond to client requests. A basic example would be navigating to https://www.google.com and moving to https://www.google.com/maps. Once the client navigates to the google maps page, the google server needs to load, or serve, the client the maps page. Users typically navigate using buttons, but the functionality is still the same. These endpoints need to be re-directed or routed via the server.  
Sooooo.... How do we set up a route? 

Before we move on to a more complicated example, here is a very simple 'Hello world' example to demonstrate routing of a single-page app using express.js(This example was borrowed from https://expressjs.com/en/guide/routing.html). 

```javascript 
var express = require('express')
var app = express()

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function (req, res) {
  res.send('hello world')
})
```
High level overview of the above code: 
  1) Declaration of Dependencies(Express)
  2) Initializing a variable, app, to reference the express() method
  3) Setting up basic routing for the root endpoint('/') for the http 'get' method 
  4) Sending the client back and writing 'hello world' to the page 
  
What exactly does app.get() accomplish? Before I explain that code, here is what the code app.get() simplifies: 
```javascript 
//require the http built-in node module 
  var http = require('http'); 
  
 //declare a variable to for PORT 
  var PORT = 8080; 
  
 //create a server and handle any incoming request from the client 
  var server = http.createServer(handleRequest); 
  
 //function handleRequest to handle any incoming data from client via a conditional statement 
  function handleRequest(req, res) {
    switch(req.url){
    case "/":
    
      //sends the client the header 200('ok') and the content-type which the browser should expect
      res.writeHead(200, {"Content-Type": "text/html"});
      
      //sends the client 'Hello World' string which is rendered by the browser as a h1 header due to the specified content-         type in the line above 
      res.end("<h1>Hello World<h1>"); 
      
      break; 
     //sets a default case if none of paths requested by the client are handled above 
     default: 
    
      //similar to above, we are sending the client a 404 error header, which is to be rendered as html 
      res.writeHead(404, {"Content-Type": "text/html"}); 
      
      //sends the client '404, page not found' which is rendered by the browser as a h1 header due to the specified content-        type in the line above 
      res.end("<h1> 404, page not found</h1>"); 
    }
  }
  
  //start the server to listen to PORT 
  server.listen(PORT, function() {
    console.log("You are now listening to " + PORT); 
  }
```

Before we dive back into Express, let me go through exactly what this above code is doing: 
1. Requiring the built-in http module to access the createServer method
2. Defining a port for readablility 
3. Initializing a server variable and referencing the createServer method on the http object. Then, passing the handleRequest argument to the createServer method 
   - All incoming requests are passed to the handleRequest function 
4. Defining a switch statement to explictly define each route the user is accessing/request
   - Writing a header to indicate the data type that is being sent back to the client 
   - Sending the client back html
      - This could be any browser-supported data-type: txt/json/xml/html/css/javascript/etc...
5. Initialize the server to listen to a particular PORT (8080) --*This step is important* 
   * If you forget to have your server listen to a PORT, none of your code will work.
  
This above code gets more complicated once data is sent from the client and starts to become difficult to read.
Fortunately, express.js makes this process much easier.

#### Let's run a revisit an example serving up basic html using express.js:
```javascript 
  //Dependencies 
  //==============================
  var express = require('express'); 
  
  //Set up your express app  
  //==============================
  var app = express(); 
  var PORT = 8080; 

  //Routes 
  //==============================
  app.get('/', function(req, res) {
    res.send('Hello World'); 
  }); 
  
  //Initialize server to listen to PORT  
  //==============================
  app.listen(PORT, function() {
    console.log("Now you are listening to port " + PORT); 
  }); 
```
If that isn't enough to sell you on express.js, let me list out a few advantages to change your mind: 
1. You no longer need to explictly write a header to define the data type for the browser to render the returned data/file
2. You no longer need to contain all your routing/data handling inside of the handleRequest function
   - This can become increasingly difficult to read with a more complicated routing mechanism 
   - Additionally, any bug inside your handleRequest function will render your server-side code useless 
      - Modularization in express.js helps to prevent your entire code block from crashing in the event of a bug
3. Extended functionality via the express.js framework 

#### Now you can see why routing is much easier with express, but how about serving up files to the client? 
In the http module approach, this can be done in a rather crude way via the fs.readFile("path/to/file", function(err, data){}) method and sending the data to the client via res.end(data). 
For demonstration purposes, here is a depiction of what this handleRequest function would look like: 
```javascript 
//require the http built-in node module 
  var http = require('http'); 
  var fs = require('fs'); 
  
 //declare a variable to for PORT 
  var PORT = 8080; 
  
 //create a server and handle any incoming request from the client 
  var server = http.createServer(handleRequest);
  
  //handle requests from the client 
  function handleRequest(request, response) {
    switch(req.url) {
      case '/': 
      
        //reads the home.html using the fs.readFile method 
        fs.readFile('home.html', function(err, data) {
          if(err) throw err; 
        
          res.writeHead(200, {'Content-Type': 'text/html'}); 
          
          //sends the return data in the call-back function to the client 
          res.end(data); 
          break;
      
      //default case if none of the above cases are met 
      default: 
      
        //reads the '404.html' file 
        fs.readFile('404.html', function(err, data) {
          if(err) throw err; 
          
          //writes a header 404 to send the client with content-type html 
          res.writeHead(404, {'Content-Type': 'text/html'});
          
          //sends the returned data to the client via the returned object 'data' in the call-back function above 
          res.send(data) 
       }
     }
  }
  
  //initializes the server to listen to PORT 
  server.listen(PORT, function() {
    console.log('Now you are now listening to port: ' + PORT); 
  }); 
```

Even though the above method works, it uses a lot of code that becomes rather difficult to read if you're serving up multiple files to the client and involves an itermediate step of reading the file before sending it to the client. 

Here is an example using express to serve up the same "home.html" and "404.html": 

```javascript 
  //Dependencies 
  //=============================
  var express = require('express'); //importing express module 
  var path = require('path'); //importing path module 
  
  //Set up your express app 
  //=============================
  var app = express(); 
  var PORT = 8080; //sets a port for the server to listen to 
  
  //Routes 
  //=============================
  //responds to HTTP get methods on root('/') path 
  app.get('/', function(req, res) {
    // uses response object to send a file to user by using the path module to provide an absolute path to the index.html file 
      // the path module allows the path to the file to be compatible between operating systems 
    res.sendFile(path.join(__dirname, 'index.html'), options, function(err) {
      if(err) throw err; 
    }); 
  }); 
  
  //defines a catch-all case similar to using default in a switch statement 
  app.use(function(req, res) {
  
    //defines a 404 header to send the client while also sending a '404.html' file to the client 
    res.status(404).sendFile(path.join(__dirname, '404.html'), options, function(err) {
      if(err) throw err; 
    }); 
  }); 
  
  
  
  //Starts the server to begin listening 
  //=============================
  app.listen(PORT, function() {
    console.log('You are now listening to port: ' + PORT); // You are now listening to port: 8080
  }); 
```

A few take-aways from the above express example: 
1. No longer need to use a switch statement/conditional for routing purpose
   - This modularization prevents your entire app from crashing in the event of a bug in one your routes 
   - That specific url would still not render properly for the client 
2. You are now able to send whole files without the need to use the node file-system module 
3. How to define a 404 error: 
   - This is the first example of using app.use() in this file. 
   - At this point it is important to note that app.use() handles all HTTP methods on a specific path/domain, as opposed to app.get() which only responds to HTTP get requests
      - In this case, app.use() allows the back-end developer to respond to any HTTP request to a route that does not have a defined/handled endpoint 
