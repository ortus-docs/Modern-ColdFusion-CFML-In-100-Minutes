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

Why do we have conditional statements? Most often its to control conditional instructions, especially `if` / `else if` / `else` expressions. Let's write an example by adding a method to our `PersonalChef.cfc` class:


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