# Components, Properties, and Functions

**ColdFusion (CFML) is object-oriented**

CFML is an Object-Oriented programming language which means that all the things we interact with inside the virtual machine are objects, which in our case we will call Components (CFCs). Objects can hold data, called **properties**, and they can perform actions, called **methods **or **functions**.

> Remember that objects are not only data but data + behavior.

For an example of an object, think about **you ** as a human being. You have properties/attributes like height, weight, and eye color. You have functions/methods like walk, run, wash dishes, and daydream. Different kinds of objects have different properties and functions. Some might even just be a collection of functions (utility/static/service objects) or what are refered to as stateless objects, there is no instance data that they represent.

## Classes and Instances

In Object-Oriented programming we define classes which are abstract descriptions of a category or type of thing.  In our case, we will call them components and it defines what properties and functions all objects of that type have. You can consider to be a blueprint of your object representation.  They should have a distinct job and a single responsibility, try to avoid creating God objects.

> In object-oriented programming, a God object is an object that knows too much or does too much. The God object is an example of an anti-pattern. A common programming technique is to separate a large problem into several smaller problems (a divide and conquer strategy) and create solutions for each of them. - https://en.wikipedia.org/wiki/God_object

Let's check out an example of a simple Component, `User.cfc`

 ```java
 /**
 * I represent a user in the system
 * @author Luis Majano
 */
 component accessors="true"{
  
  /**
  * The name of the user
  */
  property name="name";
  
  /**
  * The age of the user
  */
  property name="age" type="numeric";
  
  /**
  * Constructor
  */
  function init( required name ){
   variables.name = arguments.name;
   
   return this;
  }
  
  function run(){
   // run baby, run!
  }
  
 }
 ``` 

Please check out the following articles:

* https://en.wikipedia.org/wiki/Class_(computer_programming)
* https://en.wikipedia.org/wiki/Instance_(computer_science)
* https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)
* https://en.wikipedia.org/wiki/Abstraction_(software_engineering)
* https://en.wikipedia.org/wiki/God_object
* http://www.learncfinaweek.com/week1/OOP/

### Notes of Interest
 
The attribute `accessors` in the component definition denotes that automatic getters and setters will be created for all defined properties in the object.  Also notice that the component and each property can be documented using `/** **/` notation, which is great for automatic documentation generators like [DocBox](https://www.forgebox.io/view/docbox).  Get into the habit of inline documentation, it can go a long way for automatic generators and make you look like you can document like a machine!

### Creating Instances

The _User.cfc_ component above is a representation of _any_ user or the _idea_ of a user.  In order to bring it to life we will create an instance of it, populate it with instance data and then use it.

An instance, is a copy of that blueprint that you are bringing to life that will be stored in memory and used by the language during a set of executions.  Usually via a `new` or `createObject()` keyword operation from another file, which can be a template or yet another component.

```java
// Create a new instance of the User class
user = new User( name="luis" );
// execute a function within it
user.run();
```

* See https://cfdocs.org/new and https://cfdocs.org/createobject

Please note that the `new` keyword will automatically call an object's constructor: the `init()` method.  The `createObject()` will not, you will have to call the constructor manually:

```java
user = createObject( "component", "User" ).init();
```

In later chapters we will investigate the concept of [dependency injection](/dependency-injection.md). Please also note that the `createObject()` function can also be used to create different types of objects in CFML like:

* Components
* Webservices (WSDL based)
* Java Objects
* .NET assemblies
* COM Objects
* Corba

## Constructors

Every object in theory should have a constructor method or a method that initializes the object to a **ready** state.  Even if the constructor is empty, get into the habit of creating one.

```java
function init(){
 // prepare object state, cache data, start the engines
 return this;
}
```

Note that the constructor returns the `this` scope.  This is a reference of the object itself that is returned.  You can also return `this` from **ANY** other function which allows for expressive or fluently chainable methods.

```java
function setValue( required val ){
 variables.value = arguments.val;
 return this;
}

obj
 .setValue( 'myvalue' )
 .setValue( 'otherValue' );
```

## Component Scopes

Every component has certain visibility scopes where properties, variables and functions are attached to.

* `variables` - Private scope, visible internally to the CFC only, where all `properties` are placed in by default.  Public and private function references are place here as well.
* `this` - Public scope, visible from the outside world (can break encapsulation) public function references are placed here.
* `static` - No need for a CFC instance, available as a CFC representation \(Lucee only\)

## Component Attributes

The `component` construct can also have many attributes or name-value pairs that will give it some extra functionality for SOAP/REST web services and for Hibernate ORM Persistence. Each CFML engine provides different capabilities.  You can find all of them here: https://cfdocs.org/cfcomponent. Below are the most common ones:

* `accessors` - Enables automatic getters/setters for properties
* `extends` - Provides inheritance via the path of the Component (CFC)
* `implements` - Names of the interfaces it implements
* `persistent` - Makes the object a Hibernate Entity which can be fine tuned through a slew of other attributes.
* `serializable` - Whether the component can be serialized into a string/binary format or not. Default is `true`.

```java
component accessors="true" serializable="false" extends="BaseUser"{

}
```

Please note that in CFML you can also declare these attributes via annotations in the comments section, weird, I know!

```java
/**
* My User
* @extends BaseUser
* @accessors true
* @serializable true
*/
component{

}
```

## Properties

Properties are a way to create attributes/data for your object, which can also adhere to inheritance rules.  In CFML, they can also be used to describe further capabilities for RESTFul/SOAP web services and Hibernate ORM.  If `accessors` are enabled, CFML will track those properties in the `variables` scope according to their name and create automatic getter and setter methods for those properties. (https://cfdocs.org/cfproperty)

```java
property name="firstName" default="";
property name="lastName" default="";
property name="age" type="numeric" default="0";
property name="address" type="array";
```

The `property` construct can also have different name-value pair attributes that can enhance its functionality.  You can find all of them here: https://cfdocs.org/cfproperty.  Below are the most common ones:

* `type` - A valid CFML type
* `default` - Default value when the object is created, else defaults to `null`.
* `setter` - Generate a setter method or not, defaults to true
* `getter` - Generate a getter method or not, defaults to true

Please note that in CFML you can also declare these attributes via annotations in the comments section, weird, I know!

```java
/**
* The user age
* @type numeric
* @default 0
*/
property name="age"
```

## Functions

Functions are the way to interact with objects, with no functions we have no object-oriented behaviors, no abstractions and no encapsulation.  Functions have an automatic return type of `any` which means it can return any type of variable back to a user and an automatic visibility scope of `public`.  They also can take in _ANY_ amount of arguments, which don't event have to be defined in the function signature. WOWZA!

**Declaration**

```
/**
* Hint
*/
accessType returnType function name(){}
```

**Examples**

```java
function hello(){
 return "Hola";
}

private function saveData(){

}

/**
* Check for existence
*
* @name The key to check
*/
function boolean valueExists( required name ){
 return variables.exists( arguments.name );
}
```

> Please note that you can use valid JavaDoc syntax and DocBox will document your functions and arguments as well.

### Function Access Types and Scopes

Functions can have different visibility contexts:

- `public` - Available to the component and externally
- `private` - Available only to the component that declares the
method and any components that extend the component in
which it is defined
- `package` - Available only to the component that declares the
method, components that extend the component, or any
other components in the package
- `remote` - Available to a locally or remotely executing page
or component method, or a remote client through a URL,
Flash, or a web service. To publish the function as a
web service, this option is required.

Another interesting tidbit in CFML is that the visibility determines where the function will exist within the scope of the CFC.  Remember that functions in CFML are objects themselves.  They can be added, removed, renamed even at runtime thanks to CFML being a dynamic language.

- `public,remote` - The function reference is placed in both the `this` and `variables` scope
- `private,package` - The function reference is placed in the `variables` scope

Does this mean, that I can programmatically change an object at runtime by injecting (mixing) in new methods, or removing methods, or even renaming them? HECK YES SIREE BOB!  This is the beauty of the dynamic language, you can manipulate object instances at runtime.


### Function Attributes

The `function` construct can also have many attributes or name-value pairs that will give it some extra functionality according to CFML engine.  You can find all of them here: https://cfdocs.org/cffunction. Below are the most common ones:

* `output` - Will this function send content to the output stream. Try avoiding this to `true` unless you are building libraries of some type, else you are breaking encapsulation
* `description` - A short text description of the function a part from the hint
* `returnFormat` - Format to return for remote callers
 

```java
function hello() description="" returnFormat=""{

}
```

Please note that in CFML you can also declare these attributes via annotations in the comments section:

```java
/**
* Say Hello
*
* @description A nice function
* @output false
*/
function sayHello(){

}
```

### Function Arguments

Arguments tell the function how to do their operation.  A function can receive zero or more arguments separated by commas in its declaration.

**declaration**

```js
function(
 required type name=default attribute=value,
 required type name=default attribute=value
){
}
```

All CFML functions are dynamic, meaning it can take any number of arguments without you even adding the signatures.  You can call functions by passing arguments by position or via name-value pairs or even with a structure/array of values, which will be called an `argumentCollection`.  


**example**

```js
function sayHello( target ){
 return "Hi #target#! I'm #name#";
}

function add( required a, required b ){
 return a + b;
}


// Let's call add
calculator.add( 1, 2 );
calculator.add( a=1, b=2 );

// struct collection
values = { a = 1, b = 2 };
calculator.add( argumentCollection=values );

// array collection
values = [ 1, 2 ];
calculator.add( argumentCollection=values );

```

> Please also note that you can add a-la-carte metadata or name-value pairs to each argument inline or via annotations like we have seen above.

**example with annotations**

```js
/**
* Constructor
*
* @wirebox The wirebox reference
* @wirebox.inject wirebox
*/
function init( required wirebox ){
  variables.wirebox = arguments.wirebox;
  return this;
}
```

### Function Returns

CFML functions will use the `return` keyword to return a value from the function.  A function can be marked `void` in its return type to denote that it does **not** return any value.  However, if a function has no return type or `any` and you do not return explicitly a value, then the function will automatically return `null`.

```java
void function nada(){
 // I do stuff, but return nothing
}

function nada(){
 // I do stuff, but also do not return anything
}

function add( a, b ){
 return a + b;
}
```

### Function Scopes

You must be getting into the habit of scopes by now.  Functions also has access to all Component scopes plus a few more that are only available to the function itself.

- `arguments` - Collects all the incoming arguments.  If the function is called via positional, then this will be a struct with numbers as keys. If the function is called via name-value pairs, then the struct will contain the same name-value pairs.
- `local` - A struct that contains all the variables that are ONLY defined in the functions via the `var` keyword.

### Function Var Scope or Local Scope

Each function has a `local` scope that is ONLY available for the life-time of the execution of the function.  This is where you will be defining localized variables since your object can be multi-threaded. Always Always Always plan for multi-threaded applications and make sure you var scope your variables.  Why?  Well, if you do not var scope a variable then your variable will end up in the implicit scope which is `variables`.  

```js
// Sum is not var scoped, so it will be placed in the variables scope, memory leak anyone?
function hello(){
  sum = a + b;
  
  return sum;
}

function hello( a, b ){
 var sum = a + b;
 return sum;
}

function hello( a, b ){
 local.sum = a + b;
 return local.sum;
}
```

CFML also has a weird cascading lookup for variables, so if you do not explicitly specify the scope it is in, CFML will look for it for you.





