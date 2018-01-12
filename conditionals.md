# Conditionals

## Operators 

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

Why do we have conditional statements? Most often it's to control conditional instructions, especially `if` / `else if` / `else` expressions. Let's write an example by adding a method to our `PersonalChef.cfc` class:


```java
component accessors=true{

	property name="status";

	function init(){
		status = "The water is not boiling yet.";

		return this;
	}

	function water_boiling( numeric minutes ){
		if( arguments.minutes < 7 ){
			status = "The water is not boiling yet.";
		} else if ( arguments.minutes == 7 ){
			status = "It's just barely boiling.";
		} else if ( arguments.minutes == 8 ){
			status = "It's boiling!";
		} else {
			status = "Hot! Hot! Hot!";
		}

		return this;
	}

}
```

Try this example using 5, 7, 8 and 9 for the values of minutes.

```java
chef = new PersonalChef();

for( i in [ 5, 7, 8, 9 ] ){
	chef.water_boiling( i );
	systemOutput( chef.getStatus() );
}
```

* When the minutes is 5, here is how the execution goes: Is it true that 5 is less than 7? Yes, it is, so print out the line `The water is not boiling yet.`.

* When the minutes is 7, it goes like this: Is it true that 7 is less than 7? No. Next, is it true that 7 is equal to 7? Yes, it is, so print out the line `It's just barely boiling`.

* When the minutes is 8, it goes like this: Is it true that 8 is less than 7? No. Next, is it true that 8 is equal to 7? No. Next, is it true that 8 is equal to 8? Yes, it is, so print out the line `It's boiling!.`

Lastly, when total is 9, it goes:" Is it "true" that 9 is less than 7?

No. Next, is it true that 9 is equal to 7? No. Next, is it true that 9 is equal to 8? No. Since none of those are true, execute the else and print the line `Hot! Hot! Hot!`.

An `if` block has:

* One `if` statement whose instructions are executed only if the statement is **true**
* Zero or more `else if` statements whose instructions are executed only if the statement is **true**
* Zero or one `else` statement whose instructions are executed if no `if` nor `else if` statements were **true**

Only one section of the `if / else if / else` structure can have its instructions run. If the if is **true**, for instance, CFML will never look at the `else if`. Once one block executes, that’s it.

## Ternary Operator

The ternary operator is a compact way to do an `if, else, else if` expression statements.  It is very common in other languages and can be used for a more fluent expressive conditional expression.

```
( condition ) ? trueStatement : falseStatement
```

The way it works is that the `condition` is evaluated. If it is **true**, then the true statement executed; if it is **false**, then the false statement executes.

Please note that you can chain the `trueStatement` and the `falseStatement` into more tenrary operations.  However, don't abuse it as they will look ugly and just be very complex to debug.

```java
( 1 == 1 ) ? systemOutput( "true" ) : systemOutput( "false" )
```

The output of the above statement will be..... `true` of course!

## Elvis Operator

Before Elvis we had `isDefined(), structKeyExists()` and `IF` statements to do these kind of evaluations. They work, but not very expressive or concise.

The Elvis operator is primarily used to assign the `right default` for a variable or an expression Or it is a short-hand way to do parameterization. It will allow us to set a value if the variable is `Null` or does not exist.

For instance,

```java
myName = userName ?: "Anonymous";
```

If `userName` does not exist or evaluates to `null` then the default value of the `myName` will be assigned the right part of the `?:` elvis operator -> `Anonymous`

> **Warning** The elvis operator is incredibly flawed in Adobe ColdFusion 10-11 and Lucee 4.5.  Just avoid using it if you are using those versions.  Unfortunate but true.

## Safe Navigation Operator

The safe navigation operator was introduced in Adobe ColdFusion 2016 and Lucee 5.2 and it allows for you to navigate structures by not throwing the dreaded `key not exists` exception but returning an `undefined` or `null` value.  You can then combine that with the elvis operator and create nice chainable struct navigation. For example instead of doing things like:

```java
result = "";
if( structKeyExists( var, "key" ) ){
	if( structKeyExists( var.key, "otherkey" ){
		result = var.key.otherkey;
	}
}
```

You can do things like this:

```java
result = var?.key?.otherKey ?: "";
```

The hook operator (`?`) along with the dot operator (`.`) is known as safe navigation operator(`?.`). The safe navigation operator makes sure that if the variable used before the operator is not defined or java `null`, then instead of throwing an error, the operator returns `undefined` for that particular access.


## Switch, Case, & Default

Another situation that involves conditional logic is when a single variable or expression that can have a variety of values and different statements or functions needed to be executed depending on what that value is. One way of handling this situation is with a `switch / case / default` block.

```java
switch( expression ){
	case value : [ case otherValue ] : {
		// operations
		break;
	}
	
	default : {
		// Default operations
	}
}
```

Much like how the `if` statement marks the start of an `if` block and contains one or more `else if` statements and perhaps one (and only one) `else` statement, the `switch` statement marks the start of a `switch` block and can contain multiple `case` statements and perhaps one (and only one) `default` statement. 

The main difference is that `switch / case / default` can only evaluate the resulting value of a single variable or expression, while the `if / else if / else` block lets you evaluate the `true or false` result of different variables or expressions throughout the block.

```java
switch( city ){

	case "New York":
 		region= "East Coast";
 		break;
	
	case "Los Angeles":
  		region= "West Coast";
 		break;

 	case "Phoenix":
  		region= "Phoenix";
 		break;

 	case "Cleveland" : case "Cincinnati" : {
  		region= "Midwest";
		break;
	}
 	default:
  		region="Unknown";
}
```

Please note that you can create a body for the `case` statements with curly braces.  As best practice, do so for all `case` and/or `default` blocks

## While Loops

The `while( conditional )` expression allows you to execute a code block as many times as the `conditional` expression evaluates to **true**.  This is a great way to work with queues, stacks or just simple evaluations.

```
testCondition = true;
count = 0;
while( testCondition ){
    count++;
    if( count == 5) {
        testCondition = false;
    }
}
systemOutput( count );
```

## The `==` and `=` Common Mistake

The #1 mistake people encounter when writing conditional statements is the difference between `=` and `==`.

* `=` is an assignment. It means "take what's on the right side and stick it into whatever is on the left side" (or its telling not asking.)
* `==` is a question. It means "is the thing on the right equal to the thing on the left" (or its asking not telling.)




