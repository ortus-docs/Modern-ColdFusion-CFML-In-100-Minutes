# Java Integration

CFML is compiled to [Java bytecode](https://en.wikipedia.org/wiki/Java_bytecode) and runs on the JVM.  This gives CFML a unique advantage that not only can you write CFML but you can also integrate with the running JDK libraries or any Java library you tell the engine to use.  This is great, because if there is something already written in Java, just drop it in and use it, well most of the time :\) Unless Jar loading hell occurs.

{% hint style="info" %}
CommandBox even allows you to install jar's from any endpoint into your projects: [https://commandbox.ortusbooks.com/package-management/code-endpoints/jar-via-http](https://commandbox.ortusbooks.com/package-management/code-endpoints/jar-via-http)

```bash
install "jar:https://github.com/coldbox-modules/cbox-bcrypt/blob/master/modules/bcrypt/models/lib/jbcrypt.jar?raw=true" 
```
{% endhint %}

{% embed url="https://cfdocs.org/java" %}

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/using-web-elements-and-external-objects/integrating-jee-and-java-elements-in-cfml-applications/enhanced-java-integration-in-coldfusion.html" %}

## Creating Java Objects

The easiest way to integrate with Java is to be able to instantiate Java objects or call Java static methods on objects. You will do this via the `createObject()` or the `new` operator approach.  Here is the signatures for  both approaches:

```java
createObject( "java", "java.class.path" )
new java( "class.path" ); // ACF2018 ONLY
```

Examples:

```java
// Create a Java JDK Object
var buffer = createObject( "java", "java.lang.StringBuilder" );

// Using a constructor
currentFile = createObject( "java", "java.io.File" ).init( getCurrentTemplatePath() );
writeOutput( currentFile.lastModified() );

// Invoking a static method
javaSystem = new java( "java.lang.System" );
currentTime = javaSystem.currentTimeMillis();
writeOutput( currentTime );
```

## Java Casting

You must remember that Java is a static and typed language.  CFML is not!  If you need to pass in arguments to Java functions that require native types you will have to cast them.  We will use the fancy `JavaCast()` function built-in to the language.

```java
integerObject = createObject( "java", "java.lang.Integer" );
maxInt = integerObject.max( javaCast( "int", 5 ), javaCast( "int", 6 ) );
```

The `javaCast()` method takes in two arguments:

* `type` : The type of casting
* `variable` : The value to cast

The available casting types are:

* `boolean`
* `double`
* `float`
* `int`
* `long`
* `string`
* `null`
* `byte`
* `bigdecimal`
* `char`
* `short`

If you need to cast the type but as an array then you can use the `[]` in the casting construct. Let's create a stream from an incoming list of values and cast them to an array of objects in Java.

```java
createObject( "java", "java.util.Arrays" )
    .stream(
        javaCast( "java.lang.Object[]", listToArray( arguments.target, "" ) )
    );
```

### Java Nulls

Ohh the dreaded day [nulls](null-and-nothingness.md) where created.  The variable that means that nothing exists.  If you need to pass null into Java object calls then you have two approaches to create them:

```java
// Adobe + Lucee
javaCast( "null", "" );

// Lucee only
nullValue();
```

## Loading Custom Jars/Libraries

The `createObject( "java" )` method will look into the CFML engine's class loader to discover the class you request.  If the class is not located an exception is thrown that the class could not be found.  If you want to integrate with third-party Jar's and libraries then you will need to tell the engine where to look for those classes.  There are essentially three ways to add custom libraries to the CFML engine:

1. Add the jars/libs to the CFML Lib paths. These are those obscure directories both Adobe and Lucee give you so you can drop your libraries and the engine's class loader can well, load them.  Each engine has different paths, please see their docs on the matter.  We won't cover this approach as it is incredibly rigid:
   1. [https://docs.lucee.org/guides/Various/tutorial-lucee/tutorial-java-in-lucee.html](https://docs.lucee.org/guides/Various/tutorial-lucee/tutorial-java-in-lucee.html)
   2. [https://helpx.adobe.com/coldfusion/developing-applications/using-web-elements-and-external-objects/integrating-jee-and-java-elements-in-cfml-applications/about-coldfusion-java-and-jee.html](https://helpx.adobe.com/coldfusion/developing-applications/using-web-elements-and-external-objects/integrating-jee-and-java-elements-in-cfml-applications/about-coldfusion-java-and-jee.html)
2. The `Application.cfc` allows you to declare a `this.javaSettings` struct where you can declare an array of locations of the libraries to load upon application startup with some nice modifiers.  This will allow you to store and even leverage CommandBox for the management of such jars.
3. In Lucee, the `createObject( "java" )` construct allows you to pass in a third argument which can be a location or an array of locations of libraries to class load.  This is also great for custom CFCs, task runners, or isolated class loading.

### this.javaSettings

This `Application.cfc` structure takes in 3 keys that will allow you to class load any jar/.class libraries into the running ColdFusion application:

| Key | Description |
| :--- | :--- |
| `loadPaths` | An array of paths to the directories that contain Java classes or JAR files.You can also provide the path to a JAR or a class. If the paths are not resolved, an error occurs. |
| `loadColdFusionClassPath` | Indicates whether to load the classes from the ColdFusion lib directory. The default value is false. |
| `reloadOnChange` | Indicates whether to reload the updated classes and JARs dynamically, without restarting ColdFusion. The default value is false. |
| `watchInterval` | Specifies the time interval in seconds after which to verify any change in the class files or JAR files. This attribute is applicable only if the reloadOnChange attribute is set to true. The default value is 60 seconds. |
| `watchExtensions` | Specifies the extensions of the files to monitor for changes. By default, only `.class and .jar` files are monitored. |

{% code-tabs %}
{% code-tabs-item title="Application.cfc" %}
```java
component{

    this.javaSettings = {
        loadPaths = [ "./lib", "./config/java/myjar.jar" ],
        reloadOnChange = false
    }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Once that is declared in your Application.cfc and you execute a `createobject()` with a class from those libraries, now ColdFusion will know about them and create them.

### createObject\(\) Lucee Class Loading

The other approach is to leverage the `createObject()` call to do class loading.  Please note that only Lucee supports this feature as of now.

```java
// Reference
createObject( "java", "path", array or jar location )

// Example
variables.LIB_PATH = expandPath( "/mylib/apache.jar" );
createObject( "java", "org.apache.pdfbox.pdmodel.PDDocument", variables.LIB_PATH );
```

## Dynamic Proxies

Both ColdFusion engines also allows you to create dynamic proxies from existing ColdFusion Components \(CFCs\).  What this means is that a Dynamic proxy lets you pass ColdFusion components to Java objects. Java objects can work with the ColdFusion components seamlessly as if they are native Java objects by implementing the appropriate Java interfaces.  You can even use them to simulate Java lambdas as ColdFusion components.

```java
createDynamicProxy( cfc, interfaces )
```

If you want to leverage a Java library that requires a certain type of Java object as an argument and instead of you creating that object in Java, you can see if that argument adheres to a certain interface and CFML will create a dynamic proxy that binds it.

```java
createDynamicProxy(
    new proxies.Consumer( arguments.consumer ), // create a Consumer CFC
    [ "java.util.function.Consumer" ] // match it to this interface
)

// Here is the Consumer CFC

/**
 * Functional Interface that maps to java.util.function.Consumer
 * See https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html
 */
component extends="BaseProxy"{

    /**
     * Constructor
     *
     * @f The lambda or closure to be used in the <code>accept()</code> method
     */
    function init( required f ){
        super.init( arguments.f );
        return this;
    }

    /**
     * Performs this operation on the given argument.
     */
    void function accept( t ){
		loadContext();
        variables.target( t );
    }

    function andThen( after ){}

}
```

Basically your CFC must implement the appropriate methods the interface\(s\) tell you that it needs. After that, you CFC will be Javafyied and it can be used like native Java interface implementing objects!  Enjoy!

