## What is MongoDB?
- Nosql document database
- Documents are stored in collections of documents

***What is a Document?***
- A way to organize and store data as a set of fieid-value pairs

***Collection*** 
- An organized store of documents in MongoDB

***ReplicaSets***
- Set of a few connected mongodb instances that store the same data

## How does MongoDB store data?
***JSON***
Keys in json document are also called fields in mongodb

|Pros of Json   |Cons of Json  |
|---|---|
|Friendly  |Text based   |
|Readable   |Space consuming   |
|Familiar  |Limited   |

***BSON***
Bridges the gap between binary representation and JSON format
<br>
Optimized for:
- Speed
- Space
- Flexibility

***JSON***
- Encoding => UTF-8 String
- Data Support => String, Boolean, Number, Array
- Readability => Human and Machine

***BSON***
- Encoding => Binary
- Data Support => String, Boolean, Number(Integer, Long, Float,..), Date, Raw Binary
- Readability => Machine only

### Summary
- BSON => MongoDB stores data in BSON internally and over the network
- JSON => Can be natively stored and retrieved in MongoDB
- BSON provides additional features like speed and flexibility
- Data stored as BSON and viewed as JSON in MongoDB

## Importing and Exporting Data

***Commands***
|Json   |Bson  |
|---|---|
|mongoimport |mongorestore  |
|mongoexport   |mongodump   |

***URI String***
- srv => stands for secure connection

```mongodb+srv://user:password@clusterURI.mongodb.net/database```

### Export Commands
- ```mongodump --uri "<cluster uri>"``` exports data in BSON
- ```mongoexport --uri "<cluster uri>" --collectionn=<collection-name> --out=<filename>.json``` exports data in JSON

### Import Commands
- ```mongorestore --uri "<cluster uri>" --drop dump``` imports data in BSON dump
- ```mongoimport --uri "<cluster uri>" --drop=<filename>.json``` imports data in JSON

***All Commands***
```
mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"

mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json

mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json
```

### Find Command
- `admin` and `local` are the default databases that stores users data who have admin rights
- `show dbs` => shows databases in the cluster
- `use <db_name>` => switch to db
- `show collections` => shows collections in db
- `db.<collection-name>.find("<query>")` => search query
- `db.<collection-name>.find("<query>").count()` => count of documents

***Summary***
- Use `show dbs` and `show collections` for available namespaces
- `find()` returns a cursor with documents that match the find query
- `count()` returns the number of documents that match the find query
- `pretty()` formats the documents in the cursor

## Creating and Manipulating Documents
- Every document must have a unique `_id` value
- `ObjectId()` is the default value for the `_id` field unless otherwise specified
- `db.<collection-name>.findOne()` => get one document
- `db.<collection-name>.insert({<json>})` => inserts a document
- Insert multiple documents at once `db.<collection-name>.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])`
- Use `{"ordered": false}` to disable the default ordered insert

***An example***
Which of the following commands will successfully insert 3 new documents into an empty pets collection?

- `db.pets.insert([{ "pet": "cat" }, { "pet": "dog" }, { "pet": "fish" }])` This is correct. The _id field is not specified in any of these documents, which means that it will be created for each automatically, and it will be unique.

- `db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 1, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 4, "pet": "snake" }], { "ordered": true })` This is incorrect. There is a duplicate id issue between the "cat" and "dog" documents. While there are still 3 documents with unique _id values, the duplicate key error will appear on the second document in the array, and will prevent the operation from reaching the other documents. This will happen due to the ordered nature of this insert operation.

- `db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 1, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 4, "pet": "snake" }], { "ordered": false })` This is correct. This insert is unordered, which means that each document with a unique _id value will get inserted into the collection, which would make a total of 3 inserted documents.

- `db.pets.insert([{ "_id": 1, "pet": "cat" },
                { "_id": 2, "pet": "dog" },
                { "_id": 3, "pet": "fish" },
                { "_id": 3, "pet": "snake" }])` This is correct. While there is a duplicate key error between the "fish" and "snake" documents, it occurs at the very end of the insert operation, because this insert is ordered by default. As a result the first 3 documents will get inserted and the last one will create a duplicate key error.

- `MQL` stands for mongodb query language

### Update Operations
- `updateOne()` => updates one document in the collection
- `updateMany()` => updates all documents in the collection

***Update Operators***
- `{"$inc": {"pop": 10, "<field2>": <increment value>, ...}}` increments field value by a specified amount
- `{"$set": {"pop": 10, "<field2>": <new value>, ...}}` sets field value by a new specified value
- `{"$push": {<field1>: <value1>, ...}}` adds an element to an array field

***Example***
- `db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })`
- `db.zips.updateOne({ "zip": "12534" }, { "$set": { "pop": 17630 } })`
- `db.grades.updateOne({ "student_id": 250, "class_id": 339 },
                    { "$push": { "scores": { "type": "extra credit",
                                             "score": 100 }
                                }
                     })`

### Delete Operations
- `db.<collection>.drop()` deleted the given collection
- `deleteOne(), deleteMany()` deletes documents that match a given query 

***Examples***
- `db.inspections.deleteMany({ "test": 1 })`
- `db.inspections.deleteOne({ "test": 3 })`
- `db.inspection.drop()` drops the collection

## Advanced CRUD Operations
### Comparision Operators***
- `$eq` equal to
- `$ne` not equal to
- `$gt` greater than
- `$lt` less than
- `$gte` greater than or equal to
- `$lte` less than or equal to
- `{<field>: {<operator: <value>>}}`
- `$eq` is used as the default operator when an operator is not specified

***Examples***
- `db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$ne": "Subscriber" } }).pretty()`

- `db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$eq": "Customer" }}).pretty()` 

### Logical Operators
- `$and` Match all of the specified query clauses
- `$or` At least one of the query clauses is matched
- `$nor` Fail to match both given clauses
- `$not` Negates the query requirement
- `$and` is used as the default operator when an operator is not specified
`{sector: "Mobile Food Vendor - 881", result: "Warning"}` is the same as `{"$and": [{sector: "Mobile Food Vendor - 881", {result: "Warning"}}]}`

***Example***
- `db.routes.find({ "$and": [ { "$or" :[ { "dst_airport": "KZN" },
                                    { "src_airport": "KZN" }
                                  ] },
                          { "$or" :[ { "airplane": "CR2" },
                                     { "airplane": "A81" } ] }
                         ]}).pretty()`
????


## Indexing and Aggregation Framework
