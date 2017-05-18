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