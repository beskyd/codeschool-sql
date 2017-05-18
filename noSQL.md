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
