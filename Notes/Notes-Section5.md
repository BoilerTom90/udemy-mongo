## Section 5 Notes - Schema Data Relations & Querying Documents

### this sections is mostly about storing relationships with Id values...


### update posts to have userID fronm a user

```
> db.users.find().pretty()
{ "_id" : ObjectId("5dffb589abd16d07211f0d13"), "name" : "tom", "age" : 55 }
{ "_id" : ObjectId("5dffb589abd16d07211f0d14"), "name" : "bob" }
{ "_id" : ObjectId("5dffb589abd16d07211f0d15"), "name" : "charlie" }
{
        "_id" : ObjectId("5dffc1ca57ab554ea716eebe"),
        "name" : "Ken",
        "sports" : [
                "XC",
                "Track"
        ]
}
{
        "_id" : ObjectId("5dffc45157ab554ea716eec0"),
        "myId" : ObjectId("5dffc45157ab554ea716eebf"),
        "name" : "TomTom"
}

> db.posts.updateOne({userId: 3}, {$set: {userId: ObjectId("5dffb589abd16d07211f0d13")}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.posts.findOne({userId: ObjectId("5dffb589abd16d07211f0d13")})
{
        "_id" : ObjectId("5dffb4e0a1a9d4577fabc7ce"),
        "userId" : ObjectId("5dffb589abd16d07211f0d13"),
        "id" : 21,
        "title" : "asperiores ea ipsam voluptatibus modi minima quia sint",
        "body" : "repellat aliquid praesentium dolorem quo\nsed totam minus non itaque\nnihil labore molestiae sunt dolor eveniet hic recusandae veniam\ntempora et tenetur expedita sunt"
}

### Generic example with a complex query
> db.users.updateOne({name: "Tom"}, {$set: {"phone.areaCode": "224"}})
> db.users.findOne({name: "Tom"})         
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        }
}
```

```
> db.users.updateOne({name: "Tom"}, {$set: {"address.city": "Arlington Heights"}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.findOne({name: "Tom"})         
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        },
        "ratings" : {
                "_id" : 3,
                "rating" : 300
        },
        "address" : {
                "city" : "Arlington Heights"
        }
}

> db.users.updateOne({name: "Tom"}, {$set: {"days_of_week": []}})                
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.updateOne({name: "Tom"}, {$push: {"days_of_week": "Monday"}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.findOne({name: "Tom"})                                       
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        },
        "ratings" : {
                "_id" : 3,
                "rating" : 300
        },
        "address" : {
                "city" : "Arlington Heights"
        },
        "days_of_week" : [
                "Monday"
        ]
}
 ```

 ### How to remove item from an array
 ```
> db.users.updateOne({name: "Tom"}, {$push: {"days_of_week": "Tuesday"}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.findOne({name: "Tom"})
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        },
        "ratings" : {
                "_id" : 3,
                "rating" : 300
        },
        "address" : {
                "city" : "Arlington Heights"
        },
        "days_of_week" : [
                "Monday",
                "Tuesday",
                "Tuesday"
        ]
}
> db.users.updateOne({name: "Tom"}, {$pull: {"days_of_week": "Tuesday"}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.findOne({name: "Tom"})
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        },
        "ratings" : {
                "_id" : 3,
                "rating" : 300
        },
        "address" : {
                "city" : "Arlington Heights"
        },
        "days_of_week" : [
                "Monday"
        ]
}
 ```

 ### How to delete a field from an object in a collection

```
> db.users.updateOne({name: "Tom"}, {$unset: {"days_of_week": ""}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
> db.users.findOne({name: "Tom"})
{
        "_id" : ObjectId("5e00e0884654ad44d9566ee3"),
        "name" : "Tom",
        "phone" : {
                "areaCode" : "224"
        },
        "ratings" : {
                "_id" : 3,
                "rating" : 300
        },
        "address" : {
                "city" : "Arlington Heights"
        }
}
```
