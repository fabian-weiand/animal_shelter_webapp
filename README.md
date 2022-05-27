# About the Project
The Animal Shelter Project Animal Shelter app is a software application that can work with existing database data from the animal shelters to identify and categorize available dogs for search and rescue. The application has both a web dash frontend and MongoDB backend to store and search records.

# Motivation
A customer of Global Rain identifies dogs that are good candidates for search-and-rescue training. When trained, these dogs are able to find and help to rescue humans or other animals, often in life-threatening conditions. To help identify dogs for training, the customer has reached an agreement with a non-profit agency that operates five animal shelters in the region around Austin, Texas. The customer is requesting a software application that can work with existing database data from the animal shelters to identify and categorize available dogs for search and rescue.

# Getting Started
If you would like to use this project’s module you can simply add the animal_shelter.py file to your project an import is as usual. This module is not a PyPi package and therefore cannot be used with pip. 

There are some basic parameters to initialize the database connection including the username and password for the users to authenticate mongodb. Additionally you will need to provide the port number and the database name that you have chose to use for yoru mongodb implementation. Since mongoDB can be implemented in many ways by the engineering team, it is important to note all of these key parameters.

` shelter = AnimalShelter("aacuser", "password", "localhost", 36139, "AAC")`

This module has four primary class methods to be used that fulfill the CRUD (Create, Read, Update, Delete) capabilities of the module.

### AnimalShelter.create():

The create method inserts a document into the database collection named animals. This uses the mongoDB insert_one() method for inserting a single document at a time.

`:param data`: dict type object to be inserted to the database

`:return`: bool for success or failure.

### AnimalShelter.read():

The read method will find a document in the database and return it to the caller. This uses the mongoDB find() operation to find one or many documents based on the filter.

`:param query_filter`: (Optional) dict object to filter the find command. If left blank then all documents are retieved.

`:return`: list of dict objects are returned

### AnimalShelter.update():

The update method will accept an update filter and an update set to send to the database. This method uses the mongoDB update_one() method to update a single document at a time.
`:param update_filter: a query that matches the document to update.`

`:param update: The modifications to apply to the document.`

`:param upsert: If True, perform an insert if no documents match the filter.`

`:return: bool True if modified count is 1.`

### AnimalShelter.delete():

The delete method will take a single document and delete it from the database. This uses the mongoDB delete_one() to delete a single document at a time.

`:param delete_filter`: rdict type object that is required in order to delete the document from the database

`:return`: returns a bool for success or failure

# Installation
In order to use this application there are a few set of tools that will be required. 

### Tools
MongoDB version 4.2.6
MongDB was chosen a  NoSQL database for it reliability and performance characteristics available. MongoDB is a popular NoSQL database use across the industry. Follow the MongoDB documentation to install MongoDB

JupyterNotebook
Jupyter Notebook was used as an IDE for developing the frontend components. Follow the Jupyter Notebook documentation to install this product.

Animal Shelter imported data
The data that will be used by the project was supplied by a non-profit agency and has been stored in a CSV format in the file named aac_shelter_outcomes.csv

Pymongo
Pymongo is a python package that is installed and used with this project. This package is what brokers the backend connections and queries to the mongo database.
This package can be installed with pip
`pip install pymongo`

### Setting up the Database
1.	Using mongo shell create a new database named AAC	
	use AAC

2.	Using mongo shell create a new user for read/write access of AAC database	
	db.createUser({	user: "aacuser",	pwd:passwordPrompt(), roles:[{role:"readWrite",db:"AAC"}]})

3.	Import the animal shelter data to the database
mongoimport -u “aacuser” –db AAC –authenticationDatabase “AAC” –port $MD_PORT –type=csv –headerline –file ./aac_shelter_outcomes.csv

# Usage
### Import the module
```python
from animal_shelter import AnimalShelter 
```
 
### Initialize a database connection
```python
# Initialize database connection.
# must provide all five parameters
dbo = AnimalShelter("aacuser", "password", "localhost", 52170, "AAC")
```
 
### Create a new Document
```python
# Create a new document definition
new_doc = {
    '1': 9999,
    'age_upon_outcome': '2 months',
    'animal_id': 'SammyTest',
    'animal_type': 'Dog',
    'breed': 'Newfoundland/Australian Cattle Dog',
    'color': 'Black/White',
    'date_of_birth': '2013-08-16',
    'datetime': '2013-10-20 17:46:00',
    'monthyear': '2013-10-20T17:46:00',
    'name': 'Flynn', 'outcome_subtype': '',
    'outcome_type': 'Adoption',
    'sex_upon_outcome': 'Neutered Male',
    'location_lat': 30.3822258965608,
    'location_long': -97.3215567924164,
    'age_upon_outcome_in_weeks': 9.39146825396825
}
# create this new document  in the database
result = dbo.create(new_doc)
# result will be true if created or false if failed creation
if result:
    # find the document that was just created to validate the document was in fact created
    for doc in dbo.read({"animal_id": "SammyTest"}):
        print(doc)
    print("document creation was successful")

else:
    print("test failed creating 1 new doc") 
```
 
### Read a Document
```python
# the read method will return a list of dicts and will require looping through the results
for doc in dbo.read({"animal_id": "SammyTest"}):
    print(doc)
    print("document read success")
```
 
### Update a Document
```python
dbo.update({"animal_id": "SammyTest"}, {"$set": {"animal_id": "NewSammyTest"}})
for doc in dbo.read({"animal_id": "NewSammyTest"}):
    print(doc)
    print("document update success")
```

### Delete a Document
```python
# delete method is strictly a true/false call.
if dbo.delete({"animal_id": "SammyTest"}):
    print("delete record success")
else:
    print("delete record failed")
```

 
# Web Dash Screenshots

Unfiltered
 

Filtered by Water Rescue
 


 
Filtered by Disaster Rescue or Individual Tracking
 

Filtered by Mountain or Wilderness Rescue
 
 
Reset Filters
 

Contact
Your name: Fabian Weiand
Email: Fabian.weiand@snhu.edu
