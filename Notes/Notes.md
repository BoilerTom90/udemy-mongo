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
