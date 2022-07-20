# Mongodb Notes

## CRUD Operations

find() returns a cursor and not the documents.

findOne()

insert(), insertOne(), insertMany()

update(), updateOne(), updateMany()

delete(), deleteOne(), deleteMany()

## Limits

- 100+ levels of nesting
- 16 MB per document

## Merging related documents

- Lookup
  ```
  customers.aggregate([
    {
      $lookup: {
        from: 'books'.
        localField: 'favBooks',
        foriegnField: '_id',
        as: 'favBookData',
      }
    }
   ])
  ```

## Create (Document Creation Methods)

- insertOne({})
- insertMany([{}, {}])
- insert()
- mongoimport -d cars -c carsList --drop --jsonArray

* insertMany() is ordered insert i.e., if there is any error in between then the documents before the error will be inserted.
* insertMany([], {ordered: false})
  - This will try to insert all documents even if there is an error

Write Concern

insertOne({name: "ankit" , age: 41}, {writeConcern: {w: 1, j: false, wtimeout: 200}})

default: {w: 1, j: false} // j - journal

## Atomicity

Either completely succeed or completely fail

Import -> mongoimport tv-shows.json -d movieData -c movies --jsonArray --drop

## Find

###Query selection and Projection Operators

* Query selectors - Comparison, Evaluation, Logical, Array, Element, Comments, Geospatial
* Projection - $, $elemMatch, $meta, $slice

### Comparison
* $eq, $lt, $gt, $gte, $lte, $in, $nin, $ne
* $not, $and, $or

### Element
* $exists, $type

### Evaluation
* $jsonSchema, $mod, $where(depricated and replaced with $expr), $regex, $text
Example -> 
```
db.collection.find({
  $expr : {
    $gt: [
      { $cond: 
        { if: 
          {$gte : ["$valueOne", 50]},
          then:
          {$subtract: ["$valueOne", 10]},
          else:
          "$valueOne"
        }
      },
      "$valueTwo"
    ]
  }
})
```

### Array

* $size, $elemMatch, $all

```
db.collection.find({
  hobbies: {
    $elemMatch: {title: "Sports", frequency: {$gt: 3}}
  }
})
```

## Cursors

find() method yields as cursor

## Projection

db.collection.find({genres: "Drama"}, {"genres.$" : 1})


