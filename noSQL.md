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

#### Query operations

Querying with multiple criteria
```bash
    > db.portions.find(
        {
            "vendor": "Kettlecooked",
            "ratings.strength": 5
        }
    )
```
Comparison query operators
* `$gt` -- greater than
* `$lt` -- less than
* `$gte` -- greater than ar equal to
* `$lte` -- less than or equal to
* `$ne` -- not equal to

```bash
    > db.potions.find({ "price": {"$lt": 20} })
    > db.potions.find({ "price": {"$gt": 10, "$lt": 20} })
    > db.potions.find({ "vendor": {"$ne": "Brewers"} })
```

##### Range queries on an array
Use `$elemMatch`
to make sure at least 1 element matches all criteria.
```bash
    > db.potions.find(
        {"sizes": {"$elemMatch": {"$gt": 8, "$lt": 16}}}
    )
```
**Warning!** Be Careful When Querying Arrays With Ranges. Each value in the array is checked individually. If at least 1 array value is true for each criteria,
the entire document matches.
```bash 
    {"sizes": {"$gt": 8, "$lt": 16}}
    // both criteria are met by at least one value
    // "sizes" : [2,8,16]
```
Conversely, the document will not match if only 1 criteria is met. 
```bash
    {"sizes": {"$gt": 8, "$lt": 16}}
    // will not match
    // "sizes": [32, 64, 80]
```

##### Customizing queries

find() takes a second parameter called a “projection” that we can use to specify the exact
fields we want back by setting their value to true.
```bash
    > db.potions.find(
        {"grade": {"$gte": 80}},
        {"vendor": true, "name": true}
        // all other fields but the _id are automatically set to false
    )
```
Excluding fields
```bash
    > db.potions.find(
        {"grade": {""$gte: 80}},
        {"vendor": false, "price": false}
        // all other fields are set to true
    )
```
The _id field is always returned whenever selecting or excluding fields. It’s the only field that
can be set to false when selecting other fields.
```bash
    > db.potions.find(
        {"grade": {""$gte: 80}},
        {"vendor": true, "price": true, "_id": false}
        // the only time we can mix an exclusion with selections
    )
```
Whenever projecting, we either select or exclude the fields we want — we don’t do both.
```bash
    > db.potions.find(
        {"grade": {""$gte: 80}},
        {"vendor": true, "price": false}
        // causes an error
    )
```
##### The Cursor Object

Whenever we search for documents, an object is returned from the find method called a
“cursor object.”

```bash
    > db.potions.find({"vendor": "Kettlecooked"})
    // By default, the first 20 documents are printed out
    {"_id": ObjectId(...), ... }
    {"_id": ObjectId(...), ... }
    {"_id": ObjectId(...), ... }
    ...
```
type "it" for more
```bash
    > it
    // another 20 documents are printed out
```
##### Cursor methods
Method on cursor that returns the count
of matching documents
```bash
    > db.potions.find().count()
    80
```
Sort
```bash
    > db.potions.find().sort({"price": 1})
    // use 1 to order ascending
    // use -1 to order descending
```
Basic pagination with `limit()` and `skip()` methods
```bash
    > db.potions.find().limit(3)
    // prints out only 3 first documents
    > db.potions.find().skip(4).limit(5)
    // skips first 4 documents and prints out the following 5
```
This approach can become really expensive
with large collections. 

