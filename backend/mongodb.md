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

- Query selectors - Comparison, Evaluation, Logical, Array, Element, Comments, Geospatial
- Projection - $, $elemMatch, $meta, $slice

### Comparison

- $eq, $lt, $gt, $gte, $lte, $in, $nin, $ne
- $not, $and, $or

### Element

- $exists, $type

### Evaluation

- $jsonSchema, $mod, $where(depricated and replaced with $expr), $regex, $text
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

- $size, $elemMatch, $all

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

# Aggregation Framework

### $match

```
db.collection.aggregate([
  { $match: { gender: "female" } }
])
```

### $group

```
db.collection.aggregate([
  { $match: { gender: "female" } }
  { $group: { _id: {state: "$location.state"}, totalPersons: { $sum: 1 }}}
])

db.collection.aggregate([
  { $group: { _id: { birthYear: { $isoWeekYear: "$birthdate" }, num: { $sum: 1}}}
])

db.collection.aggregate([
  { $group: { _id: { age: "$age" }, allHobbies: {$push: "$hobbies"}}}
])
```

### $sort

```
db.collection.aggregate([
  { $match: { gender: "female" } }
  { $group: { _id: {state: "$location.state"}, totalPersons: { $sum: 1 }}}
  { $sort : { totalPersons : -1 }}
])
```

### $project

```
db.collection.aggregate([
  { $project: { _id: 0, gender: 1, __FILTERS__ } }
])

db.collection.aggregate([
  { $project:
    { _id: 0, gender: 1,
      location: {
        type: "Point",
        coordinates: [
          "$location.coordinates.longitude",
          "$location.coordinates.latitude"
        ]
      }
    }
  }
])
```

### $unwind

```
db.collection.aggregate([
  { $unwind: "$hobbies" }
  { $group: { _id: { age: "$age" }, allHobbies: {$push: "$hobbies"}}}
  // $push | $addToSet
])
```

## Filters

```
fullName: { $concat: ["$name.first", " ", "$name.last"] }
fullName: { $toUpper: "$name.first" }
fullName: { $substrCP: ["$name.first", 0, 1]}
nameLength: { $strLenCP : "$name.first" }
age: { $subtract: [5, 1]}
birthdate: { $toDate: "$birthdate" }
data: { $convert: { input: "$data", to: "double", onError: 0.0, onNull: 0.0}}
scores: { $slice: ["$arrayname", 1]}
scores: { $size: "$arrayname" }
scores: {
  $filter: {
    input: "$arrayname",
    as: "sc",
    cond: {
      $gt: ["$$sc.score", 30]
    }
  }
}

```
