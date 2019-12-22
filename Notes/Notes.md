## NoSQL Sampls Syntax

```
db.test_results.insertOne({
   _id: 1,
   name: "tom",
   byear: 1964,
   grade: 100,
   passed: true
})
```

Looks a lot like Javascript...

## What is Mongo DB? 
1. A cross-platform document-oriented database program.
1. Schema-less database
1. Stores data in a JSON-like document.
1. MongoDB Inc offers cloud services called Atlas and their own GUI to manage the data called Compass

### Features
- free to use
- dynamic schema
- great scalability - horizontally
- user-friendly
- fast
- Atlas - cloud based solution
- Compass - great GUI
- Drivers for most languages
  - C/C++, C#, Java, Node.js, Perl, PHP, Python, Ruby, Scala

### To Install on Windows
Browse to mongodb download community
Select Server
Download button
use defaults in most cases
be sure to install compass -- similar to phpMyAdmin


### Configure
Create the data dir
```
mkdir \data
```

Add the mongo install area to your path
```
C:\Program Files\MongoDB\Server\4.2\bin
```

### Try some basic commands
#### Start the server, then run the CLI
```
mongod --dbpath data
mongo
> show dbs
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
>
```

## Using MongoDB in a Shell
### Createa  db called demo
### Create a couple collections. A collection is like a table in SQL

```
> use demo
switched to db demo
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> use demo
switched to db demo
> db.cars.insert({brand: "BMW"})
WriteResult({ "nInserted" : 1 })
> show dbs
admin   0.000GB
config  0.000GB
demo    0.000GB
local   0.000GB
> db.cars.insert({brand: "Subaru"})
WriteResult({ "nInserted" : 1 })
> show collections
cars
> db.cars.find()
{ "_id" : ObjectId("5dfe631ac9659ea136478737"), "brand" : "BMW" }
{ "_id" : ObjectId("5dfe6331c9659ea136478738"), "brand" : "Subaru" }
> for (i = 0; i < 20000; i++) {
... db.mydata.insert(
... {"number": i}
... )
... }
WriteResult({ "nInserted" : 1 })
> show collections
cars
mydata
> db.mydata.count()
20000
> db.mydata.find({"number": 2})     
{ "_id" : ObjectId("5dfe643cc9659ea13647873b"), "number" : 2 }
>

```

### Insert lots of data at once
```
use <dbname>
db.users.insertMany( <json structure> );
db.users.find().pretty()
db.users.find().count()
db.users.count()
```

### Importing from a file
```
$ ls
data  Notes  posts.json  products.json  README.md  users.json
$ mongoimport --db demotest --collection posts --jsonArray --file posts.json
2019-12-21T15:33:46.686-0600    connected to: mongodb://localhost/
2019-12-21T15:33:46.717-0600    100 document(s) imported successfully. 0 document(s) failed to import.

$ mongo
> use demotest
switched to db demotest
> show collections
posts
users
> db.posts.count() 
100
> db.users.count()
10
>
 ```