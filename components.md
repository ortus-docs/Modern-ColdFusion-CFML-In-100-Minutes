# Components, Properties, and Functions

**ColdFusion (CFML) is object-oriented**

CFML is an Object-Oriented programming language which means that all the things we interact with inside the virtual machine are objects, which in our case we will call Components (CFCs). Each piece of data is an object. Objects hold information, called properties/fields, and they can perform actions, called methods or functions.

> Remember that objects are not only data but data + behavior.

For an example of an object, think about **you **as a human being. You have attributes like height, weight, and eye color. You have methods like walk, run, wash dishes, and daydream. Different kinds of objects have different properties and functions.

## Classes and Instances

In Object-Oriented programming we define classes which we will call CFML **components**, which are abstract descriptions of a category or type of thing. It defines what properties and functions all objects of that type have. You can consider to be a blueprint of your object representation.  An instance, is a copy of that blueprint that you are bringing to life that will be stored in memory and used by the language.  Usually via a `new` or `createObject()` keyword operation.

```
user = new User();
user.run();
```
