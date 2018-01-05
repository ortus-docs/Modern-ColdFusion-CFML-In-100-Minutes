# Components, Properties, and Functions

**ColdFusion (CFML) is object-oriented**

CFML is an Object-Oriented programming language which means that all the things we interact with inside the virtual machine are objects, which in our case we will call Components (CFCs). Objects can hold data, called **properties**, and they can perform actions, called **methods **or **functions**.

> Remember that objects are not only data but data + behavior.

For an example of an object, think about **you ** as a human being. You have properties/attributes like height, weight, and eye color. You have functions/methods like walk, run, wash dishes, and daydream. Different kinds of objects have different properties and functions. Some might even just be a collection of functions (utility/static/service objects) or what are refered to as stateless objects, there is no instance data that they represent.

## Classes and Instances

In Object-Oriented programming we define classes which are abstract descriptions of a category or type of thing.  In our case, we will call them components and it defines what properties and functions all objects of that type have. You can consider to be a blueprint of your object representation.  They should have a distinct job and a single responsibility, try to avoid creating God objects.

> In object-oriented programming, a God object is an object that knows too much or does too much. The God object is an example of an anti-pattern. A common programming technique is to separate a large problem into several smaller problems (a divide and conquer strategy) and create solutions for each of them.

Let's check out an example of a simple Component:

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
 
* See https://en.wikipedia.org/wiki/Class_(computer_programming)
* See https://en.wikipedia.org/wiki/Instance_(computer_science)
* See http://www.learncfinaweek.com/week1/OOP/

### Notes of Interest
 
The attribute `accessors` in the component definition denotes that automatic getters and setters will be created for all defined properties in the object.  Also notice that the component and each property can be documented using `/** **/` notation, which is great for automatic documentation generators like [DocBox](https://www.forgebox.io/view/docbox).  Get into the habit of inline documentation, it can go a long way for automatic generators.

### Creating Instances

The user component above is a representation of _any_ user or the _idea_ of a user.  In order to bring it to life we will create an instance of it, populate it with instance data and then use it.

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

In later chapters we will investigate the concept of dependency injection. Please also note that the `createObject()` function is also used to create different types of objects like:

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

The `component` construct can also have many attributes or name-value pairs that will give it some extra functionality. Each CFML engine provides different capabilities.  You can find all of them here: https://cfdocs.org/cfcomponent. Below are the most common ones:

* `accessors` - Enables automatic getters/setters for properties
* `extends` - Provides inheritance
* `implements` - Names of the interfaces it implements
* `persistent` - Makes the object a Hibernate Entity which can be fine tuned through a slew of other attributes.
* `serializable` - Whether the component can be serialized into a string/binary format or not. Default is `true`.
* `



