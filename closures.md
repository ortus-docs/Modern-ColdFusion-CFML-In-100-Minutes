# Closures

> A closure is the combination of a function and the lexical environment within which that function was declared.

Remember that functions (UDFs) in CFML are objects, closures are objects as well.  So are closures and functions the same? The answer is yes and no.  The main difference between a UDF and a closure is that closures have access to their lexical environment in which they are declared.  Both functions and closures can be manipulated at runtime and can be passed around to other functions and closures or can be returned from other functions and closures. Phew!

A closure can be used in any of the following ways:

* Defined inline without giving a name. 
* They can be assigned to a variable, array item, struct, and variable scope. 
* It can be returned directly from a function.

## Assigned Closures

```java
function hello(){
    var name = "luis";

    var display = function(){
        systemOutput( name );
    }

    display();
}

hello();
```

If we execute this template via CommandBox our output will be **luis**. Meaning the `display` closure has access to its surroundings in order to display the `name` variable.  It can manipulate it, add to it, remove from it and more.

## Returned Closures
 
We can also have a function return a closure that can leverage the function's variable environment.

```java
function makeAdder( required x ){
	return function( required y ){
		return x + y;
	};
}

add = makeAdder( 1 );
systemOutput( add( 2 ) );
```

In this case the `makeAdder` creates a function that will add the passed in variable with another via a delay of execution.  You can then execute the resultant closures `add` with another number to get your calculation of `3` in this case.

**Funky!!**

## Delayed Execution

Another big advantage of leveraging closures for functional programming is that closures are the blueprint of a function and are not executed until you want to. Meaning they are useful for delaying execution and great for design patterns like: observer, filters, iterators and much more.

```java
var observe = function( val ){
    // manipulate the val and return it
    
    return val;
}

describe( "A spec suite", function(){

    it( "can do funky stuff", function(){
        // I can do funky stuff here
        
    } );

} );
```

## Closure Scopes

A closure retains a copy of variables visible at the time of its creation. The global variables (like ColdFusion specific scopes) and the local variables (including declaring or outer function's local and arguments scope) are retained at the time of a closure creation. Functions are static.

The following details the scope of closure based on the way they are defined:

| Scenario      | Scope         |
| ------------- |:-------------:|
| In a CFC function      | Closure argument scope, enclosing function local scope and argument scope, this scope, variable scope, and super scope |
| In a CFM function      | Closure argument scope, enclosing function local scope and argument scope, this scope, variable scope, and super scope|
| As function argument| Closure argument scope, variable scope, and this scope and super scope (if defined in CFC component).      |

In a closure, the following is the order of search for an unscoped variable:

- Closure's `local` scope
- Closure's `arguments` scope
- Outer function's `local` scope if available
- Owner function's `local` scope if available
- ColdFusion built-in scope

##