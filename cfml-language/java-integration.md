# Java Integration

CFML is compiled to Java bytecode and runs on the JVM.  This gives CFML a unique advantage that not only can it be written in CFML it can also integrate with the running JDK API libraries or any Java library you tell the engine to use.  This is great, because if there is something already written in Java, just drop it in and use it, well most of the time :\) Unless Jar loading hell occurs.

{% embed url="https://cfdocs.org/java" %}

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/using-web-elements-and-external-objects/integrating-jee-and-java-elements-in-cfml-applications/enhanced-java-integration-in-coldfusion.html" %}

## Creating Java Objects

The easiest way to integrate with Java is to be able to instantiate Java objects or call Java static methods on objects. You will do this via the `createObject()` or the `new` operator approach.  Here is the signatures for  both approaches:

```java
createObject( "java", "java.class.path" )
new java( "class.path" ); // ACF2018 + Lucee 5.3
```

Examples:

```java
// Create a Java JDK Object
var buffer = createObject( "java", "java.lang.StringBuilder" );

// Using a constructor
currentFile = new java( "java.io.File" ).init( getCurrentTemplatePath() );
writeOutput( currentFile.lastModified() );

// Invoking a static method
javaSystem = new java( "java.lang.System" );
currentTime = javaSystem.currentTimeMillis();
writeOutput( currentTime );
```

## Java Casting

You must remember that Java is a static and typed language.  CFML is not!  If you need to pass in arguments to Java functions that require native types you will have to cast them.  We will use the fancy `JavaCast()` function built-in to the language.

