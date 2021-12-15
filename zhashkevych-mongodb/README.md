Learning NoSQL / MongoDB with zhashkevych

## Links

[NoSQL with MongoDB Video](https://www.youtube.com/watch?v=bEpIZfOxItU)

Introductory

[MongoDB University - MongoDB Basics](https://university.mongodb.com/courses/M001/about)

[MongoDB University - MongoDB for SQL Pros](https://university.mongodb.com/courses/M100/about)

[MongoDB University - Authentication & Authorization](https://university.mongodb.com/courses/M150/about)


Intermediate

[MongoDB University - MongoDB Performance](https://university.mongodb.com/courses/M201/about)

[MongoDB University - MongoDB for Python Developers](https://university.mongodb.com/courses/M220P/about)


Advanced

[MongoDB University - Data Modeling](https://university.mongodb.com/courses/M320/about)


## Steps
Create Docker cCntainer with Mongo
```bash
docker run --name mongo_test --rm -d -e MONGO_DATABASE=admin -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=qwerty -p 27019:27017 mongo
```

Go to Mongo Container
```bash
docker exec -it <hash> /bin/bash
```


Go to Mongo
```bash
mongo -u root -p qwerty --authenticationDatabse admin
```


Show all dbs
```bash
show dbs;
```


Switch to admin db
```bash
use admin;
```


Switch to test_db
```bash
use test_db;
```


Create new document in memberships-collection in test_db
```bash
db.memberships.insertOne({
    title: "Basic 3 month",
    price: 1999,
    duration: 3
})
```

Get all documents from memberships collection
```bash
db.memberships.find({})
```


Get all documents with indents
```bash
db.memberships.find({}).pretty()
```


Get with filter - duration = 3
```bash
db.memberships.find({
    duration: 3
}).pretty()
```


Get with filter - duration >= 6
```bash
db.memberships.find({
    duration: {$gte: 6}
}).pretty()
```


Get with filter - duration >= 3
```bash
db.memberships.find({
    duration: {$gte: 3}
}).pretty()
```


Insert documents with price-object
```bash
db.memberships.insertOne({
    title: "Basic 6 month",
    duration: 6,
    price: {
        value: 3999,
        currency: "UAH"
    }
})
db.memberships.find({})

db.memberships.insertOne({
    title: "Basic 6 month",
    duration: 6,
    price: {
        value: 3999,
        currency: "UAH"
        bonuses: ['SPA', 'Massage']
    }
})

db.memberships.find({}).pretty()
```



Insert document with one parameter
```bash
db.memberships.insertOne({
    name: "Jack"
})

db.memberships.find({}).pretty()
```


Than we can use MongoDB Compass with <strong>autocompletion</strong>

Update bonuses in document with ObjectId
```bash
db.memberships.updateOne({
    _id: ObjectId('...')
},
{
    $addToSet: {
        bonuses: "SPA"
        }
});
```

Update with adding new bonus in document with ObjectId
```bash
db.memberships.updateOne({
    _id: ObjectId('...')
},
{
    $push: {
        bonuses: "SPA"
        }
});
```


Delte document with ObjectId
```bash
db.memberships.deleteOne({
    _id: ObjectId('...')
});
```
