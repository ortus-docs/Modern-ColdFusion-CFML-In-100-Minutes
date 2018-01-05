# Conditionals

Conditional statements evaluate to **true** or **false** only. The most common conditional operators are `==` (equal), `!=` (not equal), `>` (greater than), `>=` (greater than or equal to), `<` (less than), and `<=` (less than or equal to). You can also define the operators as abbreviations: `EQ, NEQ, GT, GTE, LT, and LTE`. 

```java
a = 1;
if( a == 1 )
if( a > 2 )
if( a != 2 )
if( a >= 1 )
if( a <= 1 )
```

Some instructions return a `true` or `false`, so they're used in conditional statements, for example, `IsArray` which is `true` only when the variable is an "array". Structures have an instruction named `structKeyExists()` or `exists()` which returns `true` if a key is present in a structure.  Strings can also be used for conditional operations by checking the `.length()` member function.

```java
a = [1,3]

if( isArray( a ) ){
    // work on the array
}

produce = {
    grapes     = 2,
    lemons     = 1,
    eggplants  = 6
};

if( produce.keyExists( "grapes" ) ){
    // eat a grape
    produce.grapes--;
}
```

Also integers can be evaluated as **true** or **false**. In ColdFusion, **0 (zero)** is **false **and any other integers are **true**.

```
<cfif 1>I am true so will show</cfif>

<cfif -2>I am true so will show</cfif>

<cfif 0>I am false so will not show</cfif>
```

## If, Else If, & Else

Why do we have conditional statements? Most often its to control conditional instructions, especially `if` / `else if` / `else` expressions. Let's write an example by adding a method to our **PersonalChef** class:


```java
component accessors=true{

	property name="status";

	function init(){
		status = "The water is not boiling yet.";

		return this;
	}

	function water_boiling( numeric minutes ){
		if( arguments.minutes < 7 ){
			this.status = "The water is not boiling yet.";
		} else if ( arguments.minutes == 7 ){
			this.status = "It's just barely boiling.";
		} else if ( arguments.minutes == 8 ){
			this.status = "It's boiling!";
		} else {
			this.status = "Hot! Hot! Hot!";
		}

		return this;
	}

}
```