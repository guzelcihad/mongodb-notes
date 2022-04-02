## What is MongoDB?
- Nosql document database
- Documents are stored in collections of documents

***What is a Document?***
- A way to organize and store data as a set of fieid-value pairs

***Collection*** 
- An organized store of documents in MongoDB

***ReplicaSets***
- Set of a few connected mongodb instances that store the same data

## Importing and Exporting Data
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