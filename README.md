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
In this step we will create a new database called `tododb`. For this we will use the `use` statement.

```
use tododb
```



### Express app creation

#### Initialize server directory and install express
In this step we will create an express app.
For this I will use the `server` directory.
We initialize the content (actually the `package.json` file) with npm.

```
mkdir server
cd server
npm init -y
```

We need to install Express by issuing the following command. `--save` will save the dependency to package.json.

```
npm install --save express
```

#### Additional packages
We also need additional packages.

```
npm install --save body-parser mongoose morgan babel-preset-es2015-node6 source-map-support 
```

Detailed info about these ones:
  * `body-parser` is a Node.js body parsing middleware. It reads a form's input and stores it as a javascript object accessible through req.body.
  * `morgan` is a HTTP request logger middleware for node.js
  * `babel-preset-es2015-node6` makes Node.js fully ES2015 compatible.
  * `source-map-support` a module which provides source map support for stack traces in node via the V8 stack trace API. It uses the source-map module to replace the paths and line numbers of source-mapped files with their original paths and line numbers.

#### Server implementation

Create index.js:

```
touch index.js
```

The implementation of the server looks like the following:

```javascript
var express = require('express');
var app = express();

app.get('/', (req, res) => {res.send('Hello World!')})
app.listen(3000, () => console.log('Example app listening on port 3000!'))
```
