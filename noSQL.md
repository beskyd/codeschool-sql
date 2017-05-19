## MongoDB

* open-source NoSQL database
* document oriented
* great for unstructured data, especially when you have a lot of it

**SQL:** Database => Table => Row

**MongoDB:** Database => Collection => Document


#### Access `MongoDB` through the terminal application
```bash
    mongo
    >
```
MongoDB commands go after the `>`

#### Interact with MongoDB

Regular JavaScript assignment
```bash
    > potion = {
        "name": "Invisibility",
        "vendor": "Kettlecooked"
    }
```
Access the variable to see the contents
```bash
    > potion
    {
        "name": "Invisibility",
        "vendor": "Kettlecooked"
    }
```
#### Using the shell
Switch to use the database or create if it does not exist when we write to it
```bash
    > use reviews
    switched to db reviews
```
Return the current db name
```bash
    > db
    reviews
```
Show list of commands
```bash
    > help
    db.help()
    ...
    show dbs
```
Show list of databases
```bash
    > show dbs
    test        0.078 GB
    reviews     0.078 GB
```
Inserting documents into a collection
```bash
    > db.potions.insert(
    {
        "name": "Invisibility",
        "vendor": "Kettlecooked"
    }
    )
```
Whenever we write to the database, we'll always be returned a WriteResult object that tells us if the operation was successful or not
```bash
    WriteResult({ "nInserted": 1 }) // one document successfully inserted
```
To retrieve documents from the collection use `find()` method
```bash
    db.potions.find()
```

#### Queries and data types

Find a specific potion with a query
```bash
    > db.potions.find({"name": "invisibility"})
```
We can store both integers and floats in a document
```bash
    {
        "price": 10.99,
        "score": 59
    }
```
Dates can be added using JavaScript Date object and get saved in the db as an ISODate object
```bash
    {
        "tryDate": new Date(2012, 8, 13) 
        // reads as September 13, 2012 since JavaScript months begin at  0.
    }
```
Adding an array
```bash
    {
        "ingredients": ["new toes", 42, "laughter"]
    }
```

Embedded documents
```bash
    {
        "ratings": {"strength": 2, "flavor": 5}
    }
    // an embedded doc does not require an id since it's a child of the main document
```
Array values are treated individually, which means we can query them by specifying the field
of the array and the value we’d like to find.
```bash
    > db.potions.find({"ingredients": "laughter"})
```
We can search for potions by their ratings using dot notation to specify the embedded field
we’d like to search
```bash
    > db.potions.find({"ratings.flavor": 5})
```

#### Removing and modifying documents

Delete all documents that match a query
```bash
    > db.potions.remove(
        {"name": "Love"}
    )
```
Update a price of a single potion
```bash
    > db.potions.update(
        {"name": "Love"},
        {"$set": {"price": 3.99}}
        // update operators always begin with a $
    )
```
If the update parameter consists of only field/value pairs, then everything but the _id is
replaced in the matching document.
```bash
    > db.potions.update(
        {"name": "Love"},
        {"price": 3.99 }
    )
```
##### Updating multiple documents
By default `update` method updates only the first matched document. To update multiple documents use a third parameter
```bash
    > db.potions.update(
        {"name": "Love"},
        {"$set": {"price": 3.99}},
        {"multi": true}
    )
```
Update a Document's count with the `$inc` operators
```bash
    > db.logs.update(
        {"potion": "Shrinking"},
        {"$inc": {"count": 1}}
        // if field 'count' does not exist, it gets created with the value 1.
    )
```
If we run update on a potion that does not exist, then nothing will happen. 
The `upsert` option either updates an existing document or creates a new one.
```bash
    > db.logs.update(
        {"potion": "Love"},
        {"$inc": {"count": 1}},
        {"upsert": true}
    )
```
#### Advanced modifications
Removing fields from documents
```bash
    > db.potions.update(
        {}, // match all
        {"$unset": {"color": ""}} // the value we pass does not impact the operation
        {"multi": true} // update all potions
    )
```
Updating a field name with `rename` 
```bash
    > db.potions.update(
        {},
        {"$rename": {"score": "grade"}},
        {"multi": true}
    )
```
Updating aray values by location using *dot notation*
```bash
    > db.potions.update(
        {"name": "Shrinking"},
        {"$set": {"ingredients.1": 42}} // update the array element with index 1
    )
```
Updating values without knowing position using the positional operator. **Note:** only updates the first match *per document*
```bash
    db.potions.update(
        {"ingredients": "secret"},
        {"$set": {"ingredients.$": 42}}, // the $ is a placeholder for the matched value
        {"multi": true}
    )
```
Updating an embedded value using dot notation
```bash
    > db.potions.update(
        {"name": "Shrinking"},
        {"$set": {"ratings.strength": 5}}
    )
```
##### Useful update operators

* `$max` -- Updates if new value is greater than current or inserts if empty
* `$min` -- Updates if new value is less than current or inserts if empty
* `$mul` -- Multiplies current field value by specified value. If empty, it inserts 0.

Usage : `{"$max": {"<field>": "<value>"}}`

##### Modifying Arrays

The `"$pop" operator will remove either the first (-1) or the last (1) element of an array
```bash
    > db.potions.update(
        {"name": "Shrinking"},
        {"$pop": {"categories": 1}} // removes the last element
        {"$pop": {"categories": -1}} // removes the first element
    )
```
The `$push` operator will add a value to the end of an array
```bash
    > db.potions.update(
        {"name": "Shrinking"},
        {"$push": {"categories": "budget"}}
    )
```
The `$addToSet` operator will add the value to the end of an array unless it is already present
```bash
    > db.potions.update(
        {"name": "Shrinking"},
        {"$addToSet": {"categories": "budget"}}
    )
```
The `$pull` operator will remove any instance of a value from ana array
```bash
    > db.potions.update(
        {"name": "Shrinking"},
        {"$pull": {"categories": "tasty"}}
    )
```



