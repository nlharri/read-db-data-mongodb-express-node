# Reading MongoDB database

## What is this?
This is a tutorial and example app for which reads data from a MongoDB database. The app is using MongoDB, Express.js and Node.js

### Technologies and frameworks
MongoDB is a free and open-source cross-platform document-oriented NoSQL database program, which uses JSON-like documents with schemas.
Express.js is a minimalist web application framework or Node.js.
Node.js is an open-source, cross-platform JavaScript run-time environment that executes JavaScript code server-side. (Source: Wikipedia)

## Building the app

### MongoDB installation

First we need to install MongoDB. In this article I will install it by downloading community server from mongodb.com.
Let's extract the tgz file. I will use `~/mongodb` to store the mongodb runtime.

```
cd ~
tar -zxvf mongodb-osx-ssl-x86_64-3.6.3.tgz
mv mongodb-osx-ssl-x86_64-3.6.3 mongodb
```

MongoDB (`mongod`) defaults the database location to `/data/db/`.
Let's create this directory, and change its permissions for our username (`id -un` gets the username):

```
sudo mkdir -p /data/db
sudo chown -R `id -un` /data/db
```

Now we can start the MongoDB server (`mongod`):
```
cd ~
~/mongodb/bin/mongod
```

### Data preparation
In a different shell window we need to start the MongoDB shell (`mongo`).
```
~/mongodb/bin/mongo
```

In the MongoDB shell we can issue commands to the mongodb server to manipulate data in the database.

MongoDB stores data records in collections and the collections in databases. 
In this step we will create a new database called `tododb`. For this we will use the `use` statement.

```
use tododb
```

(The shell will return `switched to db tododb`.)

We will create a new collection todolist in this `tododb` database, and a new document in this todolist collection. The following command will create the collection as well as the new document.

```
db.todolist.insertOne({description: 'Register for the marathon', details: 'it must be done until 4.7.2018'})
```

The shell returns something like the following:

```
{
	"acknowledged" : true,
	"insertedId" : ObjectId("5ac8fbc723f00907d3d1be99")
}
```

This means the document was created successfully. You can query this by the following command:
```
db.todolist.find()
```

Result:
```
{ "_id" : ObjectId("5ac8fbc723f00907d3d1be99"), "description" : "Register for the marathon", "details" : "it must be done until 4.7.2018" }
```

Or in a formatted way:
```
db.todolist.find().pretty()
```

Result:
```
{
	"_id" : ObjectId("5ac8fbc723f00907d3d1be99"),
	"description" : "Register for the marathon",
	"details" : "it must be done until 4.7.2018"
}
```

Let's add another entry to the `todolist`:

```
db.todolist.insertOne({description: 'Get money from ATM', details: '100 USD'})
```


### Express app creation

In this step we will create an express app.
For this I will use the `server` directory.
We initialize the content (actually the `package.json` file) with npm.

```
mkdir server
cd server
npm init -y
```

We need to install Express by issuing the following command. `--save` will save the dependency to package.json. We will also install `mongoose` which is a MongoDB object modeling tool designed to work in an asynchronous environment.

```
npm install --save express mongoose
```

### Express server implementation: without db read

Create index.js:

```
touch index.js
```

The implementation of the server looks like the following. This needs to be put to `index.js`

```javascript
var express = require('express');
var app = express();

app.get('/', (req, res) => {res.send('Hello World!')})
app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

The app can be run by issuing the following command from the `server` directory:
```
node index.js
```

And then in the web browser enter `localhost:3000` to the URL.

### Express server implementation: implementing db read using mongoose

For this we need to enhance index.js. The final code looks like the following.
```
var express = require('express');
// Include mongoose in the project
var mongoose = require('mongoose');
var app = express();

// Connect to the tododb database
mongoose.connect('mongodb://localhost/tododb');

// We would like to get notified if we are connected to the db or if the connection failed.
mongoose.connection.on('error', function(err) {
  console.log('mongoose connection error: ' + err);
}
mongoose.connection.once('error', function(err) {
  console.log('mongoose connection error: ' + err);
}

app.get('/', (req, res) => {res.send('Hello World!')})
app.listen(3000, () => console.log('Example app listening on port 3000!'))
```


## What are Future Plans for this Project?
Further development plans:
  * Implementing a REST API to query and change the db data
  * Using React to display TODO data
  * Enhance the app to be able to manupulate the data
