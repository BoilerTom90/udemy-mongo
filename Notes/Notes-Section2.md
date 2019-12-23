## Notes from Section 2 - CRUD
### CLI Reference:
https://docs.mongodb.com/manual/reference/mongo-shell/  
https://docs.mongodb.com/ecosystem/drivers/

## Drop DBs and Collections
```
> use <database>
> db.dropDatabase()
> db.<collectionName>.drop()
```

### Creating Data
insert() -- may be deprecated with some drivers.
insertOne()

```
$ mongo
> db.users.insert([{name: "Tom"}, {name: "Bob"}])
BulkWriteResult({
      "writeErrors" : [ ],
      "writeConcernErrors" : [ ],
      "nInserted" : 2,
      "nUpserted" : 0,
      "nMatched" : 0,
      "nModified" : 0,
      "nRemoved" : 0,
      "upserted" : [ ]
})

> db.users.insertOne({name: 'Jimmy'})
{
      "acknowledged" : true,
      "insertedId" : ObjectId("5dfe97dd4b61f86cc8d5ca99")
}
> db.users.insertMany([{name: 'Jimmy'}, {name: 'Johns'}])
{
      "acknowledged" : true,
      "insertedIds" : [
               ObjectId("5dfe98444b61f86cc8d5ca9a"),
               ObjectId("5dfe98444b61f86cc8d5ca9b")
      ]
}
```

## Find and FindOne methods
### Main Points
- Syntax
  - db.\<collectionName\>.find([args])
  - db.\<collectionName\>.findOne([args])
- FindOne only returns first match
- Find returns all matches
- Find is deprecated in all major drivers
```
> db.users.insert({name: "Rico", age: 18})
WriteResult({ "nInserted" : 1 })
> db.users.find({age: 18})
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 18 }
> db.users.findOne({age: 18})
{
        "_id" : ObjectId("5dffa7cbf3379dfd57064509"),
        "name" : "Rico",
        "age" : 18
>  db.users.findOne({})   ## Just returns the first match                                                         
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom" }
> db.users.find({})   
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom" }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob" }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy" }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy" }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns" }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 18 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 18 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew" }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob" }}
```

## The Update Methods
```
> show dbs
admin     0.000GB
config    0.000GB
demo      0.001GB
demotest  0.000GB
example   0.000GB
local     0.000GB
> use demo
switched to db demo
> show collections
cars
mydata
users
> db.users.update({_id : ObjectId("5dffa870f3379dfd5706450c")}, {$set: {age: 34}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.find()                                                                 
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom" }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob" }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy" }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy" }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns" }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 18 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 18 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew" }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob", "age" : 34 }
> db.users.updateOne({_id : ObjectId("5dffa870f3379dfd5706450c")}, {$set: {age: 45}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

> db.users.updateMany({}, {$set: {age: 45}})
{ "acknowledged" : true, "matchedCount" : 9, "modifiedCount" : 8 }
>  db.users.find()     
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom", "age" : 45 }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob", "age" : 45 }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns", "age" : 45 }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob", "age" : 45 }
>
```

## The Save Methods
### Key Points
- Save is not like sql update. It will either insert a new record or replace an existing one. So, if you don't specify all of the fields
for a record that already exists, the existing record will have those omitted fields removed.
### NOTE
- MongoDB deprecates the db.collection.save() method. Instead use db.collection.insertOne() or db.collection.replaceOne() instead.

```
> db.users.find()
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom", "age" : 45 }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob", "age" : 45 }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns", "age" : 45 }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob", "age" : 45 }
> db.users.save({_id: ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom", "age" : 999})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.find()
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "name" : "Tom", "age" : 999 }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob", "age" : 45 }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns", "age" : 45 }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob", "age" : 45 }

> db.users.save({_id: ObjectId("5dfe95924b61f86cc8d5ca97"), "age" : 9999})                
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.find()
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "age" : 9999 }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob", "age" : 45 }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns", "age" : 45 }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob", "age" : 45 }
>
```

## The Delete Methods 

### Drop A collection
```
db.collection.drop()
db.collection.remove()
```

### Delete records from a collection based on condition
```
> db.users.find()
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "age" : 9999 }
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca98"), "name" : "Bob", "age" : 45 }
{ "_id" : ObjectId("5dfe97dd4b61f86cc8d5ca99"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9a"), "name" : "Jimmy", "age" : 45 }
{ "_id" : ObjectId("5dfe98444b61f86cc8d5ca9b"), "name" : "Johns", "age" : 45 }
{ "_id" : ObjectId("5dffa7cbf3379dfd57064509"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450a"), "name" : "Rico", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450b"), "name" : "Matthew", "age" : 45 }
{ "_id" : ObjectId("5dffa870f3379dfd5706450c"), "Name" : "Bob", "age" : 45 }
> db.users.deleteMany({age: {$lt: 46}})
{ "acknowledged" : true, "deletedCount" : 8 }
> db.users.find()
{ "_id" : ObjectId("5dfe95924b61f86cc8d5ca97"), "age" : 9999 }
> db.users.deleteOne({_id: ObjectId("5dfe95924b61f86cc8d5ca97")})
{ "acknowledged" : true, "deletedCount" : 1 }
> db.users.find()
> db.users.deleteMany({})
{ "acknowledged" : true, "deletedCount" : 0 }
>
```

## More Complex Queries
```
> db.users.insertMany([{name: "tom"}, {name: "bob"}, {name: "charlie"}])
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("5dffb589abd16d07211f0d13"),
                ObjectId("5dffb589abd16d07211f0d14"),
                ObjectId("5dffb589abd16d07211f0d15")
        ]
}
> db.users.updateOne({name: "tom"}, {$set: {age: 55}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.find()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
>
```

### How to use the find with dot notation
```
> db.users.find({name: "Tom"}).pretty()
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        },
        "ratings" : {
                "_id" : 3,
                "rating" : 300
        }
}

> db.users.findOne({name: "Tom"}).name
Tom
> db.users.findOne({name: "Tom"}).ratings
{ "_id" : 3, "rating" : 300 }
> db.users.findOn
e({name: "Tom"}).ratings.rating
300
>

```