### Section 6 Notes - Indexes and Aggregation

### Analyzing query/find time:
```
> db.posts.find({title: "autem hic labore sunt dolores incidunt"}).explain("executionStats")
{
        "queryPlanner" : {
                "plannerVersion" : 1,
                "namespace" : "demotest.posts",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "title" : {
                                "$eq" : "autem hic labore sunt dolores incidunt"
                        }
                },
                "winningPlan" : {
                        "stage" : "COLLSCAN",
                        "filter" : {
                                "title" : {
                                        "$eq" : "autem hic labore sunt dolores incidunt"
                                }
                        },
                        "direction" : "forward"
                },
                "rejectedPlans" : [ ]
        },
        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 1,
                "executionTimeMillis" : 1,
                "totalKeysExamined" : 0,
                "totalDocsExamined" : 100,
                "executionStages" : {
                        "stage" : "COLLSCAN",
                        "filter" : {
                                "title" : {
                                        "$eq" : "autem hic labore sunt dolores incidunt"
                                }
                        },
                        "nReturned" : 1,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 102,
                        "advanced" : 1,
                        "needTime" : 100,
                        "needYield" : 0,
                        "saveState" : 0,
                        "restoreState" : 0,
                        "isEOF" : 1,
                        "direction" : "forward",
                        "docsExamined" : 100
                }
        },
        "serverInfo" : {
                "host" : "ICSLP318",
                "port" : 27017,
                "version" : "4.2.2",
                "gitVersion" : "a0bbbff6ada159e19298d37946ac8dc4b497eadf"
        },
        "ok" : 1
}
>
```

### Creating an Index

Create the index and notice the difference in the number of documents examined and the time

#### Ascending Index
```
> db.posts.createIndex({title: 1})
> db.posts.find({title: "autem hic labore sunt dolores incidunt"}).explain("executionStats").executionStats
{
        "executionSuccess" : true,
        "nReturned" : 1,
        "executionTimeMillis" : 6,
        "totalKeysExamined" : 1,
        "totalDocsExamined" : 1,
        "executionStages" : {
                "stage" : "FETCH",
                "nReturned" : 1,
                "executionTimeMillisEstimate" : 0,
                "works" : 2,
                "advanced" : 1,
                "needTime" : 0,
                "needYield" : 0,
                "saveState" : 0,
                "restoreState" : 0,
                "isEOF" : 1,
                "docsExamined" : 1,
                "alreadyHasObj" : 0,
                "inputStage" : {
                        "stage" : "IXSCAN",
                        "nReturned" : 1,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 2,
                        "advanced" : 1,
                        "needTime" : 0,
                        "needYield" : 0,
                        "saveState" : 0,
                        "restoreState" : 0,
                        "isEOF" : 1,
                        "keyPattern" : {
                                "title" : 1
                        },
                        "indexName" : "title_1",
                        "isMultiKey" : false,
                        "multiKeyPaths" : {
                                "title" : [ ]
                        },
                        "isUnique" : false,
                        "isSparse" : false,
                        "isPartial" : false,
                        "indexVersion" : 2,
                        "direction" : "forward",
                        "indexBounds" : {
                                "title" : [
                                        "[\"autem hic labore sunt dolores incidunt\", \"autem hic labore sunt dolores incidunt\"]"     
                                ]
                        },
                        "keysExamined" : 1,
                        "seeks" : 1,
                        "dupsTested" : 0,
                        "dupsDropped" : 0
                }
        }
}

```

### Default Index
- the field called "_id"

### Compound Index

```
> db.users.createIndex({name:1, "ratings.rating": 1})
{
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 2,
        "ok" : 1
}

> db.users.getIndexes()
[
        {
                "v" : 2,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "demotest.users"
        },
        {
                "v" : 2,
                "key" : {
                        "name" : 1,
                        "ratings.rating" : 1
                },
                "name" : "name_1_ratings.rating_1",
                "ns" : "demotest.users"
        }
]

```
From above, notice that it was smart enough to use the first index which was just the name when building the index that contained name and ratings.rating.


### Joining Collections

```
> db.users.aggregate([{$lookup: {from: "posts", localField: "_id", foreignField: "userId", as: "usersPosts"}}]).pretty()
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
        "usersPosts" : [ ]
}
```