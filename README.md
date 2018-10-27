This repository creates 2 RESTful web services using nodejs to demonsrate contract testing using pactjs. The "accounts" service acts as the consumer and the "users" service is a producer.

The "account" service is called with necessary parameters which in turns calls the "users" service to fetch the resources from the database which in this case is a cloud hosted MongoDB instance.

How to use the repo?
- clone the repo
- create a mongodb sandbox on [mLab](https://mlab.com/databases/mongo-sandbox) and update the connection string in config.json. It should look like *mongodb://<dbuser>:<dbpassword>@ds123963.mlab.com:23963/mongo-sandbox*

    **or**
- run a docker instance of mongodb using the following steps
    - `docker pull mongo`
    - `docker run --name <name of db> --restart=always -d -p 27017:27017 mongo mongod`
    now update the connection string in config.json. It should be *mongodb://localhost:27017*
- execute the command - `npm install` (run npm --version to verify whether npm is installed)
- execute the command - `npm start`

The server now runs at http://localhost:3000

How to create a new account?
- Send a POST request with the JSON object in the body
    e.g. url - http://localhost:3000/accounts/
        body - {"id": "1000", "name": "lorem epsum", "email": "lorem.epsum@dolor.com"}

How to get all accounts from database?
- Send a GET request with no parameter e.g. http://localhost:3000/accounts/

How to get an account by its ID?
- Send a GET request with id as the parameter e.g. http://localhost:3000/accounts/2

### Detailed Explanation

We are using the following npm modules to develop the web service
   - **express** - the defacto framework to develop nodejs based web application
   - **mongoose** - the nodejs based ORM to manage interaction with MongoDB
   - **body-parser** - a middleware to parse our data sent through http requests
   - **requests** - to make http requests such as GET, POST, DELETE etc to another webservice 
   - **connect-timeout** - defines the timeout for an http call

app.js - used just to configure the app. It defines the javascript program to be executed when an endpoint is called
e.g. app.js contains the following line
```
var UserController = require('./user/UserController');
app.use('/users', UserController);
```

**server.js** - defines the port express server listening on

**db.js** - defines the mongodb instance the mongoose ORM connects to

**config.js** - reads the config.json for the configuration parameters

**config.json** - defines the default(*development*) configuration values and the environment specific configurations

**user/User.js** - defines the schema for *User* document. This helps mongoose exchange the data between the service and mongodb.

**user/UserController.js** - The controller service controls the flow of data into and from database. When we make a http GET request to user service to fetch all user information from MongoDB, the request is routed through `router.get(...)`.
