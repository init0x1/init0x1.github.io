---
layout: post
title: MongoDB
date: 2023-03-2
categories: [Backend, MongoDB,NoSql]
---

# MongoDB

MongoDB is a `document-oriented NoSQL database` that stores data in `JSON-like documents` with `dynamic schemas`, making it a popular choice for modern web applications. Instead of using tables and rows, like traditional relational databases, `MongoDB organizes data into collections of documents.`

`Each document can have a different structure`, which means that you don't need to define a schema upfront. This makes it easy to store and retrieve data with different structures, and allows for more flexible and agile development.

MongoDB is also designed to `scale horizontally`, meaning that you can add more servers to your cluster as your data grows, to handle larger workloads and ensure high availability.

Some of the key features of MongoDB include:

- Flexible document data model
- Automatic sharding for horizontal scaling
- Rich query language with support for advanced queries, text search, and geospatial queries
- Secondary indexes for faster queries
- Support for transactions and atomic operations
- Powerful aggregation framework for data analysis and reporting

## Node.js MongoDB Create Database

we first require the `MongoClient` class from the `mongodb` module. We then define the connection URI as `mongodb://127.0.0.1:27017`. This assumes that MongoDB is running on the same machine as the Node.js server and is listening on the default port `27017`.

We then define an `async main()` function and create a new instance of the `MongoClient` class with the connection `URI`.

Inside the try block of the `main()` function, we use `await client.connect()` to connect to the MongoDB server. We then use the `client.db()` method to create a new database named `myproject` 

Finally, we use the `client.close()` method to close the database connection.

```js
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();
    console.log('Connected to MongoDB server');

    // Create a new database named "mydb"
    const db = client.db('mydb');
    console.log('Database created!');

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}
main().catch(console.error);
```

## Creating a Collection

a collection is a `group of MongoDB documents`. It is equivalent to a `table` in a `relational` database. 

To create a new collection in a MongoDB database using Node.js, we can use the `createCollection()` method of the `Db` class from the `mongodb` module. Here's an example:

```js
const { MongoClient } = require('mongodb');

//Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // use database named "mydb"
    const db = client.db('mydb');
    
    // Create a new collection named "users"
    await db.createCollection('users');
    console.log('Collection created: users');

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}
main().catch(console.error);
```
## Insert Into Collection

To insert data into a MongoDB collection using Node.js, we can use the `insertOne()` or `insertMany()` method of the collection object. Here's an example:
```js
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // Use database named "mydb"
    const db = client.db('mydb');

    // Get the "users" collection
    const usersCollection = db.collection('users');

    // Insert a single document
    const result = await usersCollection.insertOne({ name: 'Abdo', age: 21 });
    console.log(`Inserted ${result.insertedCount} document`);

    // Insert multiple documents
    const documents = [
      { name: 'Ahmed', age: 25 },
      { name: 'Mohamed', age: 40 }
    ];
    const result2 = await usersCollection.insertMany(documents);
    console.log(`Inserted ${result2.insertedCount} documents`);

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
## Node.js MongoDB Find

## Find One 

To find a single document in a MongoDB collection using Node.js, you can use the `findOne({})` method of the MongoDB driver:

``` js
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // Use database named "mydb"
    const db = client.db('mydb');

    // Get the "users" collection
    const collection = db.collection('users');

    // Find a single document
    const user = await collection.findOne({ name: 'Abdo' });
    console.log(user);

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);

```
the result will be like this : 
``` js
{
  _id: new ObjectId("6403548bfd06d8a83fd7c847"),
  name: 'Abdo',
  age: 21
}
```
## Find All

To find all documents in a MongoDB collection using Node.js, you can use the `find()` method on a collection object. The `find() method returns a cursor which can be iterated over to access each document in the collection.`

```js
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();
    console.log('Connected to MongoDB server');

    // Use the "mydb" database
    const db = client.db('mydb');

    // Get the "users" collection
    const collection = db.collection('users');

    // Find all documents in the collection
    const cursor = collection.find();
    for await (const doc of cursor) {
      console.log(doc);
    }

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
## Find Some

To find some documents in MongoDB, we can use the `find` method with a `query object` that specifies the conditions for matching documents. The find method returns a cursor, which we can use to iterate over the matching documents.

```js 
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // Use database named "mydb"
    const db = client.db('mydb');

    // Get the "users" collection
    const usersCollection = db.collection('users');

    // Find documents that match a specific condition
    const query = { age: { $gt: 30 } }; // find users who are older than 30 years old
    const cursor = usersCollection.find(query);

    // Iterate over the matching documents and log them to the console
    await cursor.forEach((document) => {
      console.log(document);
    });

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
the `find` method with a `query object` that specifies the condition `age > 30`


## MongoDB Sort

Sorting is an important aspect of querying data in MongoDB. we can use the `sort()` method to sort the documents returned by a MongoDB query.

The `sort()` method accepts an object that defines the fields to sort on and the sort order (ascending or descending). The keys of the object correspond to the fields to sort on, and the values are either` 1 ` for `ascending order` or `-1` for `descending order.`
Here is an example of using the `sort()` method to sort documents in a MongoDB collection in descending order based on the `age` field:

```js
const { MongoClient } = require('mongodb');

const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    await client.connect();

    const db = client.db('mydb');
    const collection = db.collection('users');

    const result = await collection.find().sort({ age: -1 }).toArray();

    console.log(result);
  } catch (err) {
    console.log(err);
  } finally {
    await client.close();
  }
}

main().catch(console.error);
```
We call the `find()` method to retrieve all documents in the collection and pass the `sort()` method to sort the documents in `descending order based on the age field`. Finally, we call the `toArray()` method to convert the result cursor to an array of documents and log the result to the console.

## MongoDB Delete

you can delete one or more documents from a collection using the `deleteOne()` and `deleteMany()` methods respectively.

The `deleteOne()` method deletes the `first document that matches the specified criteria,` while the `deleteMany()` method deletes `all documents that match the specified criteria.`

Here's an example of how to use the `deleteOne()` method:

```js
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // Use database named "mydb"
    const db = client.db('mydb');
    
    // Get the "users" collection
    const collection = db.collection('users');

    // Delete the first document where the "name" field equals "Abdo"
    const result = await collection.deleteOne({ name: 'Abdo' });
    console.log(`${result.deletedCount} document(s) deleted`);

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
And here's an example of how to use the `deleteMany()` method:
```js
const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // Use database named "mydb"
    const db = client.db('mydb');
    
    // Get the "users" collection
    const collection = db.collection('users');

    // Delete all documents where the "age" field is greater than or equal to 30
    const result = await collection.deleteMany({ age: { $gte: 30 } });
    console.log(`${result.deletedCount} document(s) deleted`);

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
## MongoDB Drop

The `drop()` method in MongoDB is used to remove a `collection or a database from the server`. Here's an example of using the drop method to delete a collection in Node.js:
```js
const { MongoClient } = require('mongodb');

// Define the connection URL and the database name
const uri = 'mongodb://localhost:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();

    // Use database named "mydb"
    const db = client.db('mydb');

    // Delete a collection named "users"
    await db.collection('users').drop();
    console.log('Collection deleted: users');

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
## MongoDB Update

updating a document can be done using the `updateOne()` or `updateMany()` method. The `updateOne()` method `updates a single document that matches the specified filter`, while the `updateMany()` method `updates multiple documents that match the specified filter.`

Here's an example of how to use `updateOne()` to update a single document in a collection:
```js

const { MongoClient } = require('mongodb');

// Define the connection URL
const uri = 'mongodb://127.0.0.1:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    // Connect to the MongoDB server
    await client.connect();
    console.log('Connected to MongoDB server');

    // Use database named "mydb"
    const db = client.db('mydb');

    // Get the "users" collection
    const usersCollection = db.collection('users');

    // Update the first user document that matches the filter
    const filter = { name: 'Abdo' };
    const update = { $set: { age: 22 } };
    const result = await usersCollection.updateOne(filter, update);

    console.log(`${result.modifiedCount} document(s) updated`);

  } catch (err) {
    console.log(err);
  } finally {
    // Close the database connection
    await client.close();
  }
}

main().catch(console.error);
```
we can use the `updateMany()` method to update multiple documents that match the specified filter. The syntax is similar to `updateOne()`, except that `updateMany()` `updates all documents that match the filter, not just the first one.`

## MongoDB Limit 

`limit()` is used to limit the number of documents returned by a query. The `limit()` method is used in conjunction with `find() or findOne()` to return only the specified number of documents.

Here is an example of using limit() in Node.js to limit the number of documents returned by a query:
```js
const { MongoClient } = require('mongodb');

const uri = 'mongodb://localhost:27017/mydb';

async function main() {
  const client = new MongoClient(uri);

  try {
    await client.connect();
    const database = client.db('mydb');
    const collection = database.collection('users');

    // Find all documents in the collection and limit the result to 2 documents
    const result = await collection.find().limit(2).toArray();
    console.log(result);
    
  } catch (err) {
    console.log(err);
  } finally {
    await client.close();
  }
}

main().catch(console.error);
```
