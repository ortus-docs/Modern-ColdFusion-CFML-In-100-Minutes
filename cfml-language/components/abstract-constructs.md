# Abstract Constructs

The abstract constructs can be both used in Lucee and Adobe 2018.  The main goal of abstraction is to handle complexities by hiding/encapsulating unnecessary details from other users.  Abstraction is implemented in most languages by defining a class that has methods, properties & constructors.  

Abstract will allow you to define two contexts of operation:

1. Components
2. Functions

![](../../.gitbook/assets/abstract-class-vs-interface.png)

## Abstract Components

An abstract component allows you to make a template or blueprint for a component that will be eventually inherited from, so the inheriting class doesn't have to implement all of the methods.  Therefore, abstract classes cannot be instantiated but only extended.

Abstract classes can have both abstract and concrete methods defined within it.  Abstract methods have no body, they are just declared, much like interfaces.  Usually, you would do this to satisfy an interface declaration.  In my years of experience, abstract classes usually go hand in hand with interfaces and usually implement [strategy patterns](https://en.wikipedia.org/wiki/Strategy_pattern).

We would suggest that if you define abstract components that you add the prefix `Abstract` to the component name as best practice: `AbstractAnimal, AbstractLogger, AbstractPerson`. This goes a long way to help with readability and standards.

{% hint style="info" %}
In an inheritance hierarchy the first non-abstract class should implement **all** the abstract methods. 
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="AbstractAnimal.cfc" %}
```java
/**
 * An abstract animal class
 */
abstract component implements="IAnimal"{
	
	property animalSize;
	property animalType;
	
	function init( animalSize, animalType ){
		
		variables.animalSize = arguments.animalSize;
		variables.animalType = arguments.animalType;
		
		return this;
	}
	
	/**
	 * Shortcut for getting the animal size
	 */
	function getSize(){
		return variables.animalSize;
	}
	
	abstract function eat( any prey="" )
	abstract function makeNoise()
	abstract function poop()
	
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Abstract Functions

As you can see from the example above, abstract functions can be defined ONLY in an abstract component.  These functions are demarcated as abstract so inherited components can implement them.  You can have as many abstract functions in your abstract component and you can also have as many concrete functions as well:

{% code-tabs %}
{% code-tabs-item title="AbstractLogger.cfc" %}
```java
abstract component implements="ILogger"{
   
    /**
	 * Min logging level
	 */
	property name="levelMin" type="numeric";

	/**
	 * Max logging level
	 */
	property name="levelMax" type="numeric";

	/**
	 * Appender properties
	 */
	property name="properties" type="struct";
	
	/**
	 * Write an entry into the appender. You must implement this method yourself.
	 *
	 * @logEvent The logging event to log
	 */
	abstract function logMessage( required coldbox.system.logging.LogEvent logEvent )
	
	/**
	 * Setter for level min
	 *
	 * @throws AbstractAppender.InvalidLogLevelException
	 */
	AbstractAppender function setLevelMin( required levelMin ){
		// Verify level
		if( this.logLevels.isLevelValid( arguments.levelMin ) AND arguments.levelMin lte getLevelMax() ){
			variables.levelMin = arguments.levelMin;
			return this;
		} else {
			throw(
				message = "Invalid Log Level",
				detail  = "The log level #arguments.levelMin# is invalid or greater than the levelMax (#getLevelMax()#). Valid log levels are from 0 to 5",
				type    = "AbstractAppender.InvalidLogLevelException"
			);
		}
	} 
	
	/**
	 * Utiliy to send to output to console.
	 *
	 * @message Message to send
	 * @addNewLine Add a line break or not, default is yes
	 */
	private function out( required message, boolean addNewLine=true ){
		if( arguments.addNewLine ){
			arguments.message &= chr( 13 ) & chr( 10 );
		}
		createObject( "java", "java.lang.System" ).out.println( arguments.message );
	}
            
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
Only **abstract** components can contain **abstract** functions.
{% endhint %}



