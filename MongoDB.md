MongoDB
=======


## Mongo shell commands

* `use {database}` selects a database
* `db` shows the current database's name
* `db.help()` shows a list of commands
* `show dbs` shows a list of dbs
* `it` show the next results if more than 20


## Inserting

```
db.recipes.insert(
  {
    "name": "Chocolate cake",
    "author": {
      "name": "Master Chief",
      "email": "master.chief@example.com"
    },
    "taste": "good",
    "color": "black",
    "score": 0,
    "ingredients": ["Chocolate", "Butter", "Sugar", "Secret"]
    "categories": ["yummy", "fat"],
    "portions": ["2, 4, 8, 12"],
    "createdAt": new Date(2015, 8, 9), // 8th month is september
  }
)
```

* The `recipes` collection will be inserted if it don't already exists.
* Mongo will add an `_id` field if not specified

## Find

* `db.recipes.find()` returns all recipes
* `db.recipes.find({ name: "Chocolate cake" })` searches by specific field
* `db.recipes.find({ ingredients: "Chocolate" })` searches by array value
* `db.recipes.find({ "author.name": "Master Chief" })` searches by subdocument
value
* `db.recipes.find({ name: "Chocolat cake", "author.name": "Master Chief" })`
searches by multiple values

* Queries are case sensitive

### Operators

* `$max` updates if new value is greater than current or inserts if empty
* `$min` updates if new value is less than current or inserts if empty
* `$mul` multiples the current value by the value supplied (0 if empty)
* `{"$pop": {"categories": 1}}` removes the first element in array
* `{"$pop": {"categories": -1}}` removes the last element in array
* `{"$push": {"categories": "dessert"}}` adds an element to the array
* `{"$addToSet": {"categories": "dessert"}}` adds to array unless already exists
* `{"$pull": {"categories": "dessert"}}` removes all instances form the array

### Comparison operators

* `$gt`: greather than
* `$lt`: lesser than
* `$gte`: greater than or equal to
* `$lte`: lesser than or equal to
* `$ne`: not equal to (or array not containing)

```
db.recipes.find( {"score": { "$gt": 50, "$lt": 75 }} )
```
Returns recipes whose score are between 50 and 75

```
db.recipes.find( {"portions" { "$elemMatch": { "$gt": 5, "$lt": 9 } }} )
```
Returns recipes whose `portions` array contains at least one element matching
at least one of the criteria

* `{}` as query parameter updates all potions

### Update value in array

```
db.recipes.update(
  { "ingredients": "Secret" },
  {
    "$set": {
      "ingredients.$": "Cinnamon" // Replace the "secret" ingredient
    }
  }
)
```

### Projections

Used to specify the exact fields we want to return

* `.find({}, { "name": true, "author": true })` returns the `name`, `author`
and `_id` fields
* `.find({}, { "createdAt": false })` returns every fields except `createdAt`

### Cursor methods

* `.find().count()` returns the number for results
* `.find().sort({ "price": 1 })` sorts the results in ascending order
* `.find().sort({ "price": -1 })` sorts the results in descending order
* `.find().limit(3)` returns the 3 first results
* `.find().skip(3).limit(3)` returns the 3 nexts results


## Update

```
db.recipes.update(
  {"name": "Chocolate cake"}, // query parameter
  // update parameter
  {
    "$set": { "taste": "Very good" }, // updates a field
    "$inc": { "score": 1 }, // increments a field value
    "$unset": { "color": "" } // remove a field from the document
    "$rename": { "score": "grades" } // rename a field
  },
  // options
  {  
    "multi": true // performs update on all matched document
    "upsert": true // creates document if it doesn't exists
  }
)
```

### Update value in a embedded document

```
db.recipes.update(
  { "name": "Chocolat cake" },
  {
    "$set": {
      "author.name": "Clément" // Replace the "secret" ingredient
    }
  }
)
```

## Delete

`db.recipes.remove({ name: "Chocolate cake" })` to find and remove a document

* `.remove()` will remove all matching documents


## BSON

Documents are persisted in a formated called BSON. Like JSON, it can store
Strings, Numbers, Boolean, Array, Objects or Null value but also specific types
like ObjectID() and ISODate()
