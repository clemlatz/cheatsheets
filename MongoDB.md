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
    "createdAt": new Date(2015, 8, 9), // 8th month is september
    "ingredients": ["Chocolate", "Butter", "Sugar"]
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
value.

* Queries are case sensitive


## BSON

Documents are persisted in a formated called BSON. Like JSON, it can store
Strings, Numbers, Boolean, Array, Objects or Null value but also specific types
like ObjectID() and ISODate()
