MongoDB
=======


## Shell commands

* `use {database}` selects a database
* `db` shows the current database's name
* `db.help()` shows a list of commands
* `show dbs` shows a list of dbs


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
    "createdAt": new Date(2015, 8, 9), // 8th month is september
  }
)
```

* The `recipes` collection will be inserted if it don't already exists.
* Mongo will add an `_id` field if not specified

## Find

* `db.recipes.find()` returns all recipes
* `db.recipes.find({ name: "Chocolate cake" })` search by specific field
* `db.recipes.find({ ingredients: "Chocolate" })` search by array value
* `db.recipes.find({ "author.name": "Master Chief"})` search by subdocument
value

* Queries are case sensitive

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

### Operators

* `$max` updates if new value is greater than current or inserts if empty
* `$min` updates if new value is less than current or inserts if empty
* `$mul` multiples the current value by the value supplied (0 if empty)
* `{"$pop": {"categories": 1}}` removes the first element in array
* `{"$pop": {"categories": -1}}` removes the last element in array
* `{"$push": {"categories": "dessert"}}` adds an element to the array
* `{"$addToSet": {"categories": "dessert"}}` adds to array unless already exists
* `{"$pull": {"categories": "dessert"}}` removes all instances form the array 

## Delete

`db.recipes.remove({ name: "Chocolate cake" })` to find and remove a document

* `.remove()` will remove all matching documents


## BSON

Documents are persisted in a formated called BSON. Like JSON, it can store
Strings, Numbers, Boolean, Array, Objects or Null value but also specific types
like ObjectID() and ISODate()
