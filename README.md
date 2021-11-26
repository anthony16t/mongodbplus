# A mondodb wrapper

### Initialize module

``` python
db_name = 'testDatabase'
db = MongodbPlus(db_name)
```
### Setting collection name
You can set collection name using the .setCollectionName()
``` python
colName = 'users'
db.setCollectionName(colName)
```
Or passing parameter collectionName
``` python
colName = 'users'
db.findOne(collectionName=colName)
db.findMany(collectionName=colName)
```
If .setCollectionName() was set you don't need to pass parameter collectionName
unless you are inserting or deleting from another collection.
### Drop collection
``` python
collationName='fakeUsers'
db.dropCollection(collationName)
```
### Count number of documents in a collection
``` python
queryFilter={}
db.count(queryFilter,collectionName='users')
```
### Get all databases
``` python
databases = db.showDatabases()
```
### Insert one document
``` python
documentData = {
    'name':'anthony',
    'age':22
}
# check for duplicate key before inserting document
db.insertOne(documentData,checkDuplicate=True,duplicateKey='username',collectionName='users')
# follow collection schema, schema is create when first
# document is inserted in collection
# change it in json file __defaults__.json
db.insertOne(documentData,checkSchema=True,collectionName='users')
```
### Insert many document
``` python
documentData = [
    {
        'name':'anthony',
        'age':22
    },
    {
        'name':'jake',
        'age':18
    }
]
# check for duplicate key before inserting document
db.insertMany(documentData,checkDuplicate=True,duplicateKey='username',collectionName='users')
# follow collection schema, schema is create when first
# document is inserted in collection
# change it in json file __defaults__.json
db.insertOne(documentData,checkSchema=True,collectionName='users')
```
### Get all collections in current database
``` python
collections = db.showCollections()
```
### Find one document from collection
``` python
queryFilter = {'name':'anthony'}
user = db.findOne(queryFilter)
```
Or
``` python
queryFilter = {'name':'anthony'}
user = db.findOne(queryFilter,collectionName='users')
```
### Find many documents from collection
``` python
queryFilter = {'name':'anthony'}
user = db.findMany(queryFilter)
```
Or
``` python
queryFilter = {'name':'anthony'}
user = db.findMany(queryFilter,collectionName='users')
```
### Delete one document
``` python
queryFilter = {'name':'anthony'}
db.deleteOne(queryFilter,collectionName='fakeUsers')
```
### Delete many documents
``` python
queryFilter = {'name':'anthony'}
db.deleteMany(queryFilter,collectionName='fakeUsers')
```
### Update one document
``` python
queryFilter = {'name':'anthony'}
newData = {'name':'tony','initial':'a'}
db.updateOne(queryFilter,newData,collectionName='users')
```
### Update many documents
``` python
queryFilter = {'name':'anthony'}
newData = {'name':'tony','initial':'a'}
db.updateMany(queryFilter,newData,collectionName='users')
```
### Add key books to all documents in users collection
``` python
Key_Value = { 'books':'list' }
db.addKey(Key_Value,collectionName='users')
```
### Remove key books from all documents in users collection
``` python
Key_Value = { 'books':'list' }
db.removeKey(Key_Value,collectionName='users')
```
### pipeline: this pipeline will group all username where user is over the age 21
``` python
pipeLine = [{
        '$group' : {'_id' : "$username", 'total' : {'$sum' : 1}},
        '$sort':{'age':{'$gt':21}}
}]
db.aggregate(pipeLine,collectionName='users')
```
### Export database
``` python
exportToPath = './backups/'
db.export(exportToPath)
```
### Import database
This will delete any database with testDatabase name
``` python
databasePath = './backups/testDatabase.json'
db.Import(databasePath)
```