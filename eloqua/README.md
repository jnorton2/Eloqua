# Eloqua Python Connector
Python 3 library for using the Eloqua API
___ 

[Eloqua API Documentation Here](https://docs.oracle.com/cloud/latest/marketingcs_gs/OMCAC/rest-endpoints.html)


## Features

### Manage your connection
``` python
elq = EloquaConnection(COMPANY, USERNAME, PASSWORD)
```

### CRUD actions with Eloqua Assets (Custom Objects, Forms, Emails, Landing Pages)

``` python
dog_owner_custom_object = CustomObject()
dog_owner_custom_object.raw_data = {
    "name":        "TEST_Dog_Owner",
    "description": "Testing Custom Object for dog owners",
    "fields":      [
        {
            "name":        "DogBreed",
            "displayType": "text",
            "dataType":    "text"
        },
        {
            "name":        "StartOfOwnership",
            "displayType": "text",
            "dataType":    "date"
        },
        {
            "name":        "AgeAtStartOfOwnership",
            "displayType": "text",
            "dataType":    "number"
        },
        {
            "name":        "DogColor",
            "displayType": "text",
            "dataType":    "text"
        },
        {
            "name":        "DogName",
            "displayType": "text",
            "dataType":    "text"
        }
    ]
}
elq.create(dog_owner_custom_object)

```

### Generate Usable Models from Custom Objects
Generate model code. Useful for when you have data stored in Eloqua like a database. 
``` python
object_code = elq.generate_custom_object_code("Dog_Owner_0003", class_name="DogOwner")
print (object_code)
```
Yields:
``` python
class DogOwner(CustomObjectModel):
    NAME = "Dog_Owner_0003"
    PARENT_ID = "501"
    FIELDS = ["DogName1", "Breed1", "DateOfOwnership1"]
    ID_FIELD_MAP = {
        "123" : "DogName1",
        "124" : "Breed1",
        "125" : "DateOfOwnership1"
    }
    DogName1 = None
    Breed1 = None
    DateOfOwnership1 = None

```

#### CRUD actions with Custom Object Data Models
``` python
first_dog_owner = elq.get(DogOwner, record_id='1', 
    params={
        "depth":"complete"
    }
)

all_corgi_dog_owners = elq.get_list(DogOwner, 
    params={
        "search" : "Breed1='Corgi'"
    }
)

first_dog_owner.name = "New Dog Name"

elq.update(first_dog_owner)

elq.delete(first_dog_owner)

```

###Variable Paths
The `paths.py` holds all the paths used for interaction with the API. 
Sometimes, a certain API versions works better than others so this is a place where you can change that. 

```python
# Login
DEFAULT_LOGIN_URL = "https://login.eloqua.com/id"

# Forms
FORM_CREATE_PATH = "/api/REST/2.0/assets/form"
FORM_DELETE_PATH = "/api/REST/2.0/assets/form/{id}"
FORM_UPDATE_PATH = "/api/REST/2.0/assets/form/{id}"
FORM_GET_PATH = "/api/REST/2.0/assets/form/{id}"
FORM_GET_LIST_PATH = "/api/REST/2.0/assets/forms"

# Custom Objects
CUSTOM_OBJECT_CREATE_PATH = "/api/REST/2.0/assets/customObject"
CUSTOM_OBJECT_DELETE_PATH = "/api/REST/2.0/assets/customObject/{id}"
CUSTOM_OBJECT_UPDATE_PATH = "/api/REST/2.0/assets/customObject/{id}"
CUSTOM_OBJECT_GET_PATH = "/api/REST/2.0/assets/customObject/{id}"
CUSTOM_OBJECT_GET_LIST_PATH = "/api/REST/2.0/assets/customObjects"
...
```

### Testing
To perform testing, it is preferable to use your sandbox instance. 
1. Create a `eloqua/test/creds.py` file. An outline is provided in example_creds.py
2. Run `eloqua/test/test.py` unittests

  
## Requirements
* Python 3
