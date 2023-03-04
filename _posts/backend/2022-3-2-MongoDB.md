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
