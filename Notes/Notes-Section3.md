## Section 3 Notes - Operators
### $in & $nin
- $in (in clause)
- $nin (not in clause)

```
> db.users.find()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> db.users.find({age: {$in: [38,56]}})
> db.users.find({age: {$in: [38,56,55]}})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
> db.users.find({age: {$in: [38,56,55]}})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
> db.users.find({age: {$nin: [38,56]}})  
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> 
```

### $or & $nor
- $or (or clause)
- $nor (nor clause)
```
> db.users.find()  
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> db.users.find({$or: [{age: 55}, {age: 67}]})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
>

> db.users.find()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> db.users.find({$or: [{age: 54}]})
>
> db.users.find({$or: [{age: 54}, {age: 55}]})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
> db.users.find({$or: [{age: 54}, {age: {$ge: 55}}]})
> db.users.find({$or: [{age: 53}, {age: {$gt: 54}}]})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
```

### Other Operators
- $and
- $not/$ne
- $regex
- $sizes
- $elemMatch
```
> db.users.find()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> db.users.find({$and: [{age: 55}]})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
> db.users.find({$and: [{age: 55}, {age: ""}]})
> db.users.find({$and: [{age: 55}, {name: "tom"}]})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }

> db.users.find()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> db.users.find({age: {$ne: 55}})
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
>
```

### $regex Operator

```
> db.users.find({name: {$regex: /o/}})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
>

> db.users.find({name: {$regex: /^[a-z]*$/}})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }

> db.users.find({name: {$regex: /^[A-Z]*$/i}})
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
> 
```

### $size operator
```
> db.users.find()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
{ "_id" : ObjectId("5dffc1ca57ab554ea716eebe"), "name" : "Ken", "sports" : [ "XC", "Track" ] }
> db.users.find({sports: {$size: 2}})
{ "_id" : ObjectId("5dffc1ca57ab554ea716eebe"), "name" : "Ken", "sports" : [ "XC", "Track" ] }
>> db.users.find({sports: {$size: 2}}).pretty()
{
        "_id" : ObjectId("5dffc1ca57ab554ea716eebe"),
        "name" : "Ken",
        "sports" : [
                "XC",
                "Track"
        ]
}
```

### $elemMatch operator

