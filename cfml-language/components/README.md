# Components

**ColdFusion \(CFML\) is object-oriented, period!**

CFML is an Object-Oriented programming language which means that all the things we interact with inside the virtual machine are objects, which in our case we will call Components \(CFCs\). Objects can hold data, called **properties**, and they can perform actions, called **methods** or **functions,** they can inherit from other objects, they can implement interfaces, they can contain metadata, and even act as RESTFul webservices.

> Remember that objects are not only data but data + behavior.

For an example of an object, think about **you** as a human being. You have properties/attributes like height, weight, and eye color. You have functions/methods like walk, run, wash dishes, and daydream. Different kinds of objects have different properties and functions. Some might even just be a collection of functions \(utility/static/service objects\) or what are referred to as stateless objects, there is no instance data that they represent.

CFML supports not only the traditional avenues for object orientation but many unique constructs and dynamic runtime additions.  Enjoy!

## Classes and Instances

In Object-Oriented programming we define **classes** which are abstract descriptions of a category or type of thing; a blueprint. In our case, we will call them components and it defines what properties and functions all objects \(instances\) of that type have. You can consider them to be a blueprint of your object representation. They should have a distinct job and a **single** responsibility \(if possible\), try to avoid creating God objects.

> In object-oriented programming, a God object is an object that knows too much or does too much. The God object is an example of an anti-pattern. A common programming technique is to separate a large problem into several smaller problems \(a divide and conquer strategy\) and create solutions for each of them. - [https://en.wikipedia.org/wiki/God\_object](https://en.wikipedia.org/wiki/God_object)

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

* [https://en.wikipedia.org/wiki/Class\_\(computer\_programming](https://en.wikipedia.org/wiki/Class_%28computer_programming)\)
* [https://en.wikipedia.org/wiki/Instance\_\(computer\_science](https://en.wikipedia.org/wiki/Instance_%28computer_science)\)
* [https://en.wikipedia.org/wiki/Encapsulation\_\(computer\_programming](https://en.wikipedia.org/wiki/Encapsulation_%28computer_programming)\)
* [https://en.wikipedia.org/wiki/Abstraction\_\(software\_engineering](https://en.wikipedia.org/wiki/Abstraction_%28software_engineering)\)
* [https://en.wikipedia.org/wiki/God\_object](https://en.wikipedia.org/wiki/God_object)
* [http://www.learncfinaweek.com/week1/OOP/](http://www.learncfinaweek.com/week1/OOP/)
* [https://en.wikipedia.org/wiki/Mutator\_method](https://en.wikipedia.org/wiki/Mutator_method)

### Notes of Interest

The attribute `accessors` in the component definition denotes that automatic **getters \(**[**accessors**](https://en.wikipedia.org/wiki/Mutator_method)**\)** and **setters \(**[**mutators**](https://en.wikipedia.org/wiki/Mutator_method)**\)** will be created for all defined properties in the object. Also notice that the component and each property can be documented using `/** **/` notation, which is great for automatic documentation generators like [DocBox](https://www.forgebox.io/view/docbox). 

{% hint style="success" %}
Get into the habit of inline documentation, it can go a long way for automatic generators and make you look like you can document like a machine!
{% endhint %}

### Creating Instances

The _User.cfc_ component above is a representation of _any_ user or the _idea_ of a user. In order to bring it to life we will create an [**instance**](https://en.wikipedia.org/wiki/Instance_%28computer_science%29) of it, populate it with instance data and then use it.

An instance, is a copy of that blueprint that you are bringing to life that will be stored in memory and used by the language during a set of executions. Usually via a `new` or `createObject()` keyword operation from another file, which can be a template or yet another component.

```java
// Create a new instance of the User class
user = new User( name="luis" );
// execute a function within it
user.run();
```

* See [https://cfdocs.org/new](https://cfdocs.org/new) and [https://cfdocs.org/createobject](https://cfdocs.org/createobject)

Please note that the `new` keyword will automatically call an object's constructor: the `init()` method. The `createObject()` will not, you will have to call the constructor manually:

```java
user = createObject( "component", "User" ).init();
```

In later chapters we will investigate the concept of [dependency injection](../../extra-credit/dependency-injection.md). Please also note that the `createObject()` function can also be used to create different types of objects in CFML like:

* Components
* Webservices \(WSDL based\)
* Java Objects
* .NET assemblies
* COM Objects
* Corba

## Constructors

Every object in theory should have a constructor method or a method that initializes the object to a **ready** state. Even if the constructor is empty, get into the habit of creating one.

```java
function init(){
 // prepare object state, cache data, start the engines
 return this;
}
```

Note that the constructor returns the `this` scope. This is a reference of the object itself that is returned. You can also return `this` from **ANY** other function which allows for expressive or fluently chainable methods.

```java
function setValue( required val ){
 variables.value = arguments.val;
 return this;
}

obj
 .setValue( 'myvalue' )
 .setValue( 'otherValue' );
```

By default when using the `new Object()` operator, the Object's `init()` function will be called for you automatically.  If you use the `createObject()` then the `ini()` is NOT called automatically for you, you will call it explicitly.

```java
// Implicit Constructor
var obj = new Object();

// Explicit Constructor
var obj = createObject( "component", "Object" ).init();
```

### Pseudo-Constructor

The pseudo-constructor can be found in use in CFML and it's a unique beast.  Any source code that exists between the `cfcomponent` declaration and the first function is considered to be the pseudo-constructor.  This area of execution will be executed for you implicitly whenever the object is created, even before the implicit `init()` method call.  I know confusing, but here is a simple sequence: `new()/createObject() -> pseudo-constructor -> init()`

```java
component{
    // Pseudo Constructor starts here
    
    this.helper = now();   
    static {
        staticVar : 2
    };

    // Pseudo Constructor ends here
    function init(){
        return this;
    }

}
```

## Component Scopes

Every component has certain visibility scopes where properties, variables and functions are attached to.

* `variables` - Private scope, visible internally to the CFC only, where all `properties` are placed in by default.  Public and private function references are place here as well.
* `this` - Public scope, visible from the outside world \(can break encapsulation\) public function references are placed here.
* `static` - Same as in Java, ability to staticly declare variables and functions at the blueprint level and not at the instance level.  \(Lucee only\)

## Component Attributes

The `component` construct can also have many attributes or name-value pairs that will give it some extra functionality for SOAP/REST web services and for Hibernate ORM Persistence. Each CFML engine provides different capabilities. You can find all of them here: [https://cfdocs.org/cfcomponent](https://cfdocs.org/cfcomponent). Below are the most common ones:

* `accessors` - Enables automatic getters/setters for properties
* `extends` - Provides inheritance via the path of the Component \(CFC\)
* `implements` - Names of the interfaces it implements
* `persistent` - Makes the object a Hibernate Entity which can be fine tuned through a slew of other attributes.
* `serializable` - Whether the component can be serialized into a string/binary format or not. Default is `true`.

```java
component accessors="true" serializable="false" extends="BaseUser"{

}

component implements="cachebox.system.cache.ICacheProvider"{}
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

## 

