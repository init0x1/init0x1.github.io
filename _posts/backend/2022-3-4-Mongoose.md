---
layout: post
title: Mongoose
date: 2023-03-4
categories: [Backend, MongoDB,NoSql]
---

# Mongoose

Mongoose is an `Object Data Modeling (ODM)` library for Node.js and MongoDB. It provides a simple and powerful way to interact with MongoDB databases by providing a schema-based solution to model data.

First we install Mongoose, you can use the npm package manager by running the following command in your terminal:
```js
npm install mongoose
```
## Connect to MongoDB


```js
const mongoose = require('mongoose');
const url = 'mongodb://localhost/mydatabase'; // replace with your own MongoDB URI

mongoose.connect(url, (err) => {
  if (err) {
    console.log(`Error connecting to MongoDB: ${err}`);
  } else {
    console.log('Connected to MongoDB');
  }
});
```
First, the `mongoose module is imported`. Then, the `connection URL` is set to `'mongodb://localhost/mydatabase'` 

The `mongoose.connect()` method is called `with the URL and a callback function as arguments`. The `callback function` takes an `err` parameter, which will be `null` if the connection is successful, or an error object if an error occurs during the connection attempt.

If an error occurs, the callback function logs an error message to the console using `console.log()`, with the err object passed as an argument. If the connection is successful, the callback function logs a success message to the console using `console.log()`.

## Defining schemas
In Mongoose, you define a schema to describe the structure of a collection in MongoDB. Here's an example:
```js
const mongoose = require('mongoose');

// define a schema
const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: Number,
  isAdmin: {
    type: Boolean,
    default: false
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```
In this example, a `userSchema` is defined with four fields: `name`, `email`, `age`, `isAdmin`, and `createdAt`. The `email` field is marked as `required` and `unique`, and the `isAdmin` field has a default value of `false`. The `createdAt` field has a default value of the `current date and time`.


`Once a schema is defined, you can use it to create a model, which is a constructor function that provides methods for interacting with the collection`

## Creating a model

In Mongoose, we create a model by defining a schema and then passing it to the `mongoose.model()` method,The first argument to `mongoose.model()` is the `name` of the model, The resulting model is a `constructor` function that provides methods for interacting with the collection.

```js
// create a model
const User = mongoose.model('User', userSchema);

// export the model
module.exports = User;
```
Once the model is defined, you can use it to interact with the `users` collection in your MongoDB database. In the example above, the model is exported using `module.exports`, which allows it to be imported and used in other parts of your application.

## Using models
Once the `User` model is created, we can use it to perform `CRUD` (Create, Read, Update, Delete) operations on the `users` collection in MongoDB.

```js
const User = require('./models/user');

// Create a new user
const createUser = async (userData) => {
  try {
    const user = new User(userData);
    await user.save();
    console.log('User created:', user);
    return user;
  } catch (error) {
    console.error('Error creating user:', error);
    return null;
  }
};



// Read a user by ID
const getUserById = async (userId) => {
  try {
    const user = await User.findById(userId);
    console.log('User found:', user);
    return user;
  } catch (error) {
    console.error('Error getting user by ID:', error);
    return null;
  }
};

// Update a user by ID
const updateUserById = async (userId, updateData) => {
  try {
    const user = await User.findByIdAndUpdate(userId, updateData, { new: true });
    console.log('User updated:', user);
    return user;
  } catch (error) {
    console.error('Error updating user by ID:', error);
    return null;
  }
};

// Delete a user by ID
const deleteUserById = async (userId) => {
  try {
    const user = await User.findByIdAndDelete(userId);
    console.log('User deleted:', user);
    return user;
  } catch (error) {
    console.error('Error deleting user by ID:', error);
    return null;
  }
};
```
In this example, we define four functions that correspond to the four CRUD operations: `createUser`, `getUserById`, `updateUserById`, and `deleteUserById`.

Each function uses Mongoose to interact with the User model. For example, `createUser` creates a new user object based on the `userData` parameter, saves it to the database using user.`save()`, and logs the created user object to the console. If an error occurs during the creation process, an error message is logged to the console and null is returned.

Similarly, `getUserById` retrieves a user by ID using User.`findById(userId)`, logs the retrieved user object to the console, and returns it. If an error occurs during the retrieval process, an error message is logged to the console and null is returned.

The `updateUserById` function updates a user by ID using User.`findByIdAndUpdate(userId, updateData, { new: true })`. The `{ new: true }` option tells Mongoose to return the updated user object rather than the original one. If an error occurs during the update process, an error message is logged to the console and null is returned.

Finally, `deleteUserById` deletes a user by ID using User.`findByIdAndDelete(userId)`, logs the deleted user object to the console, and returns it. If an error occurs during the deletion process, an error message is logged to the console and null is returned.
