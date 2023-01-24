# Dependency Injection

> Dependency injection is the art of making work come home to you.\
> Dhanji R. Prasanna

**In ColdFusion, WireBox is the standard when it comes to Dependency Injection and Aspect Oriented Programming (AOP).**

![](../.gitbook/assets/assets\_-LA-UVvsc-e1GVkiaPQ-\_-LA-Ud03e\_n2SeqLC9Ls\_-LA-UrvQwgjVsSA\_J5pT\_WireBox.png)

WireBox alleviates the need for custom object factories or manual object creation in your ColdFusion (CFML) applications. It provides a **standardized** approach to object **construction** and **assembling** that will make your code easier to adapt to changes, easier to [test, mock](https://testbox.ortusbooks.com) and extend.

{% hint style="info" %}
You can read all about WireBox here: [https://wirebox.ortusbooks.com/](https://wirebox.ortusbooks.com/)
{% endhint %}

As software developers we are always challenged with maintenance and one ever occurring annoyance, **change**. Therefore, the more sustainable and maintainable our software, the more we can concentrate on real problems and make our lives more productive. WireBox leverages an array of metadata annotations to make your object assembling, storage and creation easy as pie! We have leveraged the power of event driven architecture via object listeners or interceptors so you can extend not only WireBox but the way objects are analyzed, created, wired and much more. To the extent that our [AOP ](https://wirebox.ortusbooks.com/aspect-oriented-programming/aop-intro)capabilities are all driven by our AOP listener which decouples itself from WireBox code and makes it extremely flexible.

## Dependency Injection Explained

We have released one of our chapters from our [CBOX202: Dependency Injection](https://www.ortussolutions.com/learn) course that deals with getting started with Dependency Injection, the problem, the benefits and the solutions. We encourage you to download it, print it, share it, digest it and learn it: [http://ortus-public.s3.amazonaws.com/cbox202-unit1-3.pdf](http://ortus-public.s3.amazonaws.com/cbox202-unit1-3.pdf)

{% hint style="success" %}
If you require any training please [contact us](https://www.ortussolutions.com/learn).
{% endhint %}

## Advantages of a DI Framework

Compared to manual Dependency Injection (DI), using WireBox can lead to the following advantages:

* You will write less boilerplate code.
* By giving WireBox DI responsibilities, you will stop creating objects manually or using custom object factories.
* You can leverage object persistence scopes for performance and scalability. Even create time persisted objects.
* You will not have any object creation or wiring code in your application, but have it abstracted via WireBox. Which will lead to more cohesive code that is not plagued with boilerplate code or factory code.
* Objects will become more testable and easier to mock, which in turn can accelerate your development by using a TDD (Test Driven Development), BDD (Behavior Driven Development) approach.
* Once WireBox leverages your objects you can take advantage of AOP or other event life cycle processes to really get funky with Object Orientation.

## Features at a Glance

Here are a simple listing of features WireBox brings to the table:

* Annotation driven dependency injection
* 0 configuration mode or a programmatic binder configuration approach via ColdFusion (No XML!)
* Creation and Wiring of or by:
  * ColdFusion Components
  * Java Classes
  * RSS Feeds
  * WebService objects
  * Constant values
  * DSL string building
  * Factory Methods
  * Providers
* Multiple Injection Styles: Property, Setter, Method, Constructor
* Automatic Package/Directory object scanning and registration
* Multiple object life cycle persistence scopes:
  * No Scope (Transients)
  * Singletons
  * Request Scoped
  * Session Scoped
  * Application Scoped
  * Server Scoped
  * CacheBox Scoped
* Integrated caching via [CacheBox](https://cachebox.ortusbooks.com), scale your objects and metadata
* Integrated logging via [LogBox](https://logbox.ortusbooks.com), never try to figure out what in the world the DI engine is doing
* Parent Factories
* Factory Method Object Creations
* Object life cycle events via WireBox Listeners/Interceptors
* Customizable injection DSL
* WireBox object providers to avoid scope-widening issues on time/volatile persisted objects
* [Aspect Oriented Programming](https://wirebox.ortusbooks.com/aspect-oriented-programming/aop-intro)

## DI Basics

### Injection Styles

There are three ways that DI frameworks can inject dependencies into object references:

1. Constructor Arguments
2. Setter Methods
3. Property Injections

Each has it's own set of pros and cons. However, the important aspect of the injection types is the order it happens. Please refer back to the list above for order reference. Here is a component leveraging all three styles, what similarities do you notice in all of them?

```java
component singleton{

    // Property injection
    property name="userService" inject="UserService";
    property name="log" inject="logbox:logger:{this}";

    /**
     * Constructor Injection
     * 
     * @myService.inject id:MyAwesomeService
     *
     */
    function init( required myService ){
        variables.myService = arguments.myService;
        return this;
    }

    function setMySecurityService( required service ) inject="SecurityService@api"{
        varaiables.securityService = arguments.service;
        return this;
    }

}
```

### Injection Annotation

All the injection styles have a marker called `inject` which can contain a value, this value is called the [Injection DSL](https://wirebox.ortusbooks.com/usage/injection-dsl). This basically tells WireBox what alias to inject into the component. The value of the injection DSL can mean different things to WireBox depending on the environment, registered custom dsl's and so much more. However, at the end of the day, it means, inject something here!!

{% hint style="info" %}
Please note that we have shown you the easiest approach to DI by leveraging annotations. If you do not like annotating your code and prefer a configuration approach; No Problem. WireBox offers a [configuration Binder](https://wirebox.ortusbooks.com/configuration/configuring-wirebox) where you can declare all your objects explicitly with all their dependencies and persistence.
{% endhint %}

Let's digest a few examples:

```groovy
property name="userService" inject="UserService";
```

The `inject="UserService"` will look for an object with that alias if it doesn't find it with the alias, it treats is like a CFC path and tries to create, inject and return that object.

```groovy
property name="log" inject="logbox:logger:{this}";
```

This inject DSL is spaced by colons (:) and tells WireBox the following:

* Look for the `logbox` DSL
  * Ask for a `logger`
    * Map it to `{this}` class

As you are starting to see, the injection DSL can be very powerful.

```java
/**
 * Constructor Injection
 * 
 * @myService.inject id:MyAwesomeService
 *
 */
function init( required myService ){
}
```

The `@myservice.inject` annotation for the constructor argument tells WireBox to look for the `id` of `MyAwesomeService` and pass it as the argument. Again, the colon is the separator of choice for DSLs.

### Persistence

WireBox by default treats all objects it creates as transient objects. Meaning it will create it, inject it and return it. After usage it get's destroyed automatically by the JVM. If you want longer persistence for the objects you can [annotate them with a scope](https://wirebox.ortusbooks.com/configuration/component-annotations/persistence-annotations) or shortcut annotations like the `singleton` annotation.

```groovy
// Transient
component{}

// Singleton
component singleton{}

// Core or Custom Scope
component scope="cachebox"
```

Available scopes are:

* `NOSCOPE` : Transient objects
* `PROTOTYPE` : Transient objects
* `SINGLETON` : Objects constructed only once and stored in the injector
* `SESSION` : ColdFusion session scoped based objects
* `APPLICATION` : ColdFusion application scope based objects
* `REQUEST` : ColdFusion request scope based objects
* `SERVER` : ColdFusion server scope based objects
* `CACHEBOX` : CacheBox scoped objects

### Usage

Ok, we have seen how to construct our objects according to DI principles, but how do we now use them? There are two modes of operation:

* Standalone
* ColdBox Application

WireBox is part of the ColdBox HMVC framework, so you can leverage DI/AOP out of the box with no configuration or startup code. If you are NOT using ColdBox then you can use WireBox in standalone mode like shown below:

```groovy
// Create the WireBox Main injector
wirebox = new wirebox.system.ioc.Injector();

// Create it with a configuration Binder
wirebox = new wirebox.system.ioc.Injector( "myBinderPath" );

// Get an object
wirebox.getInstance( "MyService" );
wirebox.getInstance( "my.path.to.Service" );
```

{% hint style="danger" %}
Please note that by default the WireBox injector once initialized it will be scoped into application scope automatically for you as: `application.wirebox`
{% endhint %}

The main method to retrieve objects is called `getInstance()` and you can see the signature below:

```java
/**
 * Locates, Creates, Injects and Configures an object model instance
 *
 * @name The mapping name or CFC instance path to try to build up
 * @dsl The dsl string to use to retrieve the instance model object, mutually exclusive with 'name
 * @initArguments The constructor structure of arguments to passthrough when initializing the instance
 * @initArguments.doc_generic struct
 * @targetObject The object requesting the dependency, usually only used by DSL lookups
 **/
function getInstance( 
  name, 
  dsl, 
  struct initArguments = structNew(), 
  targetObject="" 
)
```

That's it! You can now start rolling with dependency injection in your applications. We highly encourage you to visit our [ColdBox documentation](https://coldbox.ortusbooks.com/the-basics/models) or the standalone [WireBox documentation](https://wirebox.ortusbooks.com/) for more in-depth analysis of dependency injection. We have only touched the surface.

## Useful Resources

* [http://code.google.com/p/google-guice](http://code.google.com/p/google-guice)
* [http://www.manning.com/prasanna/](http://www.manning.com/prasanna/)
* [http://en.wikipedia.org/wiki/Aspect-oriented\_programming](http://en.wikipedia.org/wiki/Aspect-oriented\_programming)
* [http://en.wikipedia.org/wiki/Dependency\_injection](http://en.wikipedia.org/wiki/Dependency\_injection)
* [http://en.wikipedia.org/wiki/Inversion\_of\_control](http://en.wikipedia.org/wiki/Inversion\_of\_control)
* [http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)
* [http://www.theserverside.com/news/1321158/A-beginners-guide-to-Dependency-Injection](http://www.theserverside.com/news/1321158/A-beginners-guide-to-Dependency-Injection)
* [http://www.developer.com/net/net/article.php/3636501](http://www.developer.com/net/net/article.php/3636501)
* [http://code.google.com/p/google-guice/](http://code.google.com/p/google-guice/)
