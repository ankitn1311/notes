# Mongodb Notes

## CRUD Operations

find() returns a cursor and not the documents.

findOne()

insert(), insertOne(), insertMany()

update(), updateOne(), updateMany()

delete(), deleteOne(), deleteMany()

## Limits

* 100+ levels of nesting
* 16 MB per document

## Merging related documents

* Lookup
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
 
 * insertOne({})
 * insertMany([{}, {}])
 * insert()
 * mongoimport -d cars -c carsList --drop --jsonArray

- insertMany() is ordered insert i.e., if there is any error in between then the documents before the error will be inserted.
- 
