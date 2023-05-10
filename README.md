0x00. AirBnB clone - The console
-----------------------------------------------------------------------------------------------------
Group project PythonOOP
Bacground Context: First step: Write a command interpreter to manage your AirBnB objects. Each task is linked and will help you to: put in place a parent class (called BaseModel) to take care of the initialization, serialization and deserialization of your future instances, create a simple flow of serialization/deserialization: Instance <-> Dictionary <-> JSON string <-> file, create all classes used for AirBnB (User, State, City, Place…) that inherit from BaseModel, create the first abstracted storage engine of the project: File storage., create all unittests to validate all our classes and storage engine. To be able to manage the objects of our project: Create a new object (ex: a new User or a new Place), Retrieve an object from a file, a database etc…, Do operations on objects (count, compute stats, etc…), Update attributes of an object, Destroy an object
Learning Objectives: How to create a Python package, How to create a command interpreter in Python using the cmd module, What is Unit testing and how to implement it in a large project, How to serialize and deserialize a Class, How to write and read a JSON file, How to manage datetime, What is an UUID, What is *args and how to use it, What is **kwargs and how to use it, How to handle named arguments in a function
Requirements
-------------------------------------------------------------------------------------------------------------------------------------
Python Scripts: Allowed editors: vi, vim, emacs, All files will be interpreted/compiled on Ubuntu 20.04 LTS using python3 (version 3.8.5), All files should end with a new line, The first line of all files are exactly #!/usr/bin/python3, A README.md file, at the root of the folder of the project, is mandatory, Pycodestyle (version 2.8.*) should be used for the code, All files must be executable, The length of your files will be tested using wc
Python Unit Tests: Allowed editors: vi, vim, emacs, All files should end with a new line, All test files should be inside a folder tests, Use the unittest module, All test files should be python files (extension: .py), All test files and folders should start by test_, File organization in the tests folder should be the same as your project, All tests should be executed by using this command: python3 -m unittest discover tests, Also test file by file by using this command: python3 -m unittest tests/test_models/test_base_model.py
GitHub: There should be one project repository per group. 
Task 0. README, AUTHORS
----------------------------------------------------------------------------------------------------------
Write a README.md: description of the project, description of the command interpreter: how to start it, how to use it, Have an AUTHORS file at the root of your repository, listing all individuals having contributed content to the repository. Use branches and pull requests on GitHub 
Repo: GitHub repository: AirBnB_clone, File: README.md, AUTHORS
Task 1. Be pycodestyle compliant!
-------------------------------------------------------------------------------------------------------------
Write beautiful code that passes the pycodestyle checks.
Repo: GitHub repository: AirBnB_clone
Task 2. Unittests
----------------------------------------------------------------------------------------------------
All files, classes, functions must be tested with unit tests
Repo: GitHub repository: AirBnB_clone File: tests/
Task 3. BaseModel
--------------------------------------------------------------------------------------------------------------------
Write a class BaseModel that defines all common attributes/methods for other classes: models/base_model.py, Public instance attributes: id: string - assign with an uuid when an instance is created: can use uuid.uuid4() to generate unique id but don’t forget to convert to a string Use self.__dict__, only instance attributes set will be returned, a key __class__ must be added to this dictionary with the class name of the object, created_at and updated_at must be converted to string object in ISO format, create a dictionary representation with “simple object type” of our BaseModel
Repo: GitHub repository: AirBnB_clone File: models/base_model.py, models/__init__.py, tests/
Task 4. Create BaseModel from dictionary
----------------------------------------------------------------------------------------------------------------------------
Re-create an instance with this dictionary representation. <class 'BaseModel'> -> to_dict() -> <class 'dict'> -> <class 'BaseModel'> Update models/base_model.py: __init__(self, *args, **kwargs):
Repo: GitHub repository: AirBnB_clone File: models/base_model.py, tests/
 Task 5. Store first object
------------------------------------------------------------------------------------------------------------------------
Recreate a BaseModel from another one by using a dictionary representation: <class 'BaseModel'> -> to_dict() -> <class 'dict'> -> <class 'BaseModel'>, Writing the dictionary representation to a file won’t be relevant: Python doesn’t know how to convert a string to a dictionary (easily) It’s not human readable, Using this file with another program in Python or other language will be hard. So, convert the dictionary representation to a JSON string, with this format, humans can read and all programming languages have a JSON reader and writer. The flow of serialization-deserialization will be: <class 'BaseModel'> -> to_dict() -> <class 'dict'> -> JSON dump -> <class 'str'> -> FILE -> <class 'str'> -> JSON load -> <class 'dict'> -> <class 'BaseModel'> 
Write a class FileStorage that serializes instances to a JSON file and deserializes JSON file to instances: models/engine/file_storage.py Private class attributes: __file_path: string - path to the JSON file , Public instance methods: all(self): returns the dictionary __objects, new(self, obj): sets in __objects the obj with key <obj class name>.id, save(self): serializes __objects to the JSON file (path: __file_path), reload(self): deserializes the JSON file to __objects (only if the JSON file (__file_path) exists ; otherwise, do nothing. If the file doesn’t exist, no exception should be raised)
Update models/__init__.py: to create a unique FileStorage instance for your application import file_storage.py, create the variable storage, an instance of FileStorage, call reload() method on this variable
Update models/base_model.py: to link your BaseModel to FileStorage by using the variable storage import the variable storage, in the method save(self): call save(self) method of storage __init__(self, *args, **kwargs): if it’s a new instance (not from a dictionary representation), add a call to the method new(self) on storage
Repo: GitHub repository: AirBnB_clone File: models/engine/file_storage.py, models/engine/__init__.py, models/__init__.py, models/base_model.py, tests/
 Task 6. Console 0.0.1
---------------------------------------------------------------------------------------------------------------------------
Write a program called console.py that contains the entry point of the command interpreter:
Use the module cmd, class definition must be: class HBNBCommand(cmd.Cmd): command interpreter should implement: quit and EOF to exit the program, a custom prompt: (hbnb), an empty line + ENTER shouldn’t execute anything, code should not be executed when imported. Warning: end your file with: if __name__ == '__main__': HBNBCommand().cmdloop()
Repo: GitHub repository: AirBnB_clone, File: console.py
Task 7. Console 0.1
--------------------------------------------------------------------------------------------------------------------------
Update your command interpreter (console.py) to have these commands:
create: Creates a new instance of BaseModel, saves it (to the JSON file) and prints the id. If the class name is missing, print ** class name missing ** , If the class name doesn’t exist, print ** class doesn't exist ** 
show: Prints the string representation of an instance based on the class name and id. If the class name is missing, print ** class name missing ** If the class name doesn’t exist, print ** class doesn't exist ** If the id is missing, print ** instance id missing ** If the instance of the class name doesn’t exist for the id, print ** no instance found ** 
destroy: Deletes an instance based on the class name and id (save the change into the JSON file). If the class name is missing, print ** class name missing ** If the class name doesn’t exist, print ** class doesn't exist ** If the id is missing, print ** instance id missing ** If the instance of the class name doesn’t exist for the id, print ** no instance found ** 
all: Prints all string representation of all instances based or not on the class name. The printed result must be a list of strings, If the class name doesn’t exist, print ** class doesn't exist ** 
update: Updates an instance based on the class name and id by adding or updating attribute (save the change into the JSON file). 
Usage: update <class name> <id> <attribute name> "<attribute value>"
Only one attribute can be updated at the time, can assume the attribute name is valid (exists for this model). The attribute value must be casted to the attribute type, If the class name is missing, print ** class name missing **
If the class name doesn’t exist, print ** class doesn't exist ** If the id is missing, print ** instance id missing **, If the instance of the class name doesn’t exist for the id, print ** no instance found **, If the attribute name is missing, print ** attribute name missing, If the value for the attribute name doesn’t exist, print ** value missing **  All other arguments should not be used . id, created_at and updated_at cant’ be updated. Can assume they won’t be passed in the update command
Only “simple” arguments can be updated: string, integer and float. Can assume nobody will try to update list of ids or datetime, Can assume arguments are always in the right order. Each arguments are separated by a space, A string argument with a space must be between double quote, The error management starts from the first argument to the last one
Repo: GitHub repository: AirBnB_clone File: console.py
Task 8. First User
------------------------------------------------------------------------------------------
Write a class User that inherits from BaseModel: models/user.py
Public class attributes: email: string - empty string, password: string - empty string,first_name: string - empty string, last_name: string - empty string, Update FileStorage to manage correctly serialization and deserialization of User. Update your command interpreter (console.py) to allow show, create, destroy, update and all used with User. No unittests needed for the console
Repo: GitHub repository: AirBnB_clone, File: models/user.py, models/engine/file_storage.py, console.py, tests/
Task 9. More classes!
----------------------------------------------------------------------------------------------------------------------------
Write all those classes that inherit from BaseModel: State (models/state.py): Public class attributes: name: string - empty string, City (models/city.py):
Public class attributes: state_id: string - empty string: it will be the State.id, name: string - empty string, Amenity (models/amenity.py):
Public class attributes: name: string - empty string, Place (models/place.py):
Public class attributes: city_id: string - empty string: it will be the City.id user_id: string - empty string: it will be the User.id name: string - empty string, amenity_ids: list of string - empty list: it will be the list of Amenity.id later, Review (models/review.py):
Public class attributes: place_id: string - empty string: it will be the Place.id, user_id: string - empty string: it will be the User.id, text: string - empty string
Repo: GitHub repository: AirBnB_clone, File: models/state.py, models/city.py, models/amenity.py, models/place.py, models/review.py, tests/
Task 10. Console 1.0
-------------------------------------------------------------------------------------
Update FileStorage to manage correctly serialization and deserialization of all our new classes: Place, State, City, Amenity and Review, Update your command interpreter (console.py) to allow those actions: show, create, destroy, update and all with all classes created previously. No unittests needed for the console
Repo: GitHub repository: AirBnB_clone, File: console.py, models/engine/file_storage.py, tests/
Task 11. All instances by class name
-----------------------------------------------------------------------------------------------------------------
Update your command interpreter (console.py) to retrieve all instances of a class by using: <class name>.all(). No unittests needed
Repo: GitHub repository: AirBnB_clone, File: console.py
12. Count instances
----------------------------------------------------------------------------------------------------
Update your command interpreter (console.py) to retrieve the number of instances of a class: <class name>.count(). No unittests needed
Repo: GitHub repository: AirBnB_clone File: console.py
Task 13. Show
------------------------------------------------------------------------------------------------------------
Update your command interpreter (console.py) to retrieve an instance based on its ID: <class name>.show(<id>). Errors management must be the same as previously. No unittests needed
Repo: GitHub repository: AirBnB_clone File: console.py
Task. Destroy
----------------------------------------------------------------------------------
Update your command interpreter (console.py) to destroy an instance based on his ID: <class name>.destroy(<id>). Errors management must be the same as previously. No unittests needed
Repo: GitHub repository: AirBnB_clone, File: console.py
Task 15. Update
---------------------------------------------------------------------------------------------------------------------
Update your command interpreter (console.py) to update an instance based on his ID: <class name>.update(<id>, <attribute name>, <attribute value>). Errors management must be the same as previously. No unittests needed
Repo: GitHub repository: AirBnB_clone File: console.py
Task 16. Update from dictionary
------------------------------------------------------------------------------------------
Update your command interpreter (console.py) to update an instance based on his ID with a dictionary: <class name>.update(<id>, <dictionary representation>). Errors management must be the same as previously. No unittests needed
Repo: GitHub repository: AirBnB_clone File: console.py
Task 17. Unittests for the Console!
-----------------------------------------------------------------------------------------------------
Write all unittests for console.py, all features! For testing the console, you should “intercept” STDOUT of it, we highly recommend you to use with patch('sys.stdout', new=StringIO()) as f: HBNBCommand().onecmd("help show") Otherwise, re-write the console by replacing precmd by default.
Repo: GitHub repository: AirBnB_clone File: tests/test_console.py
