# Closures

> A closure is the combination of a function and the lexical environment within which that function was declared.

Remember that functions (UDFs) in CFML are objects, and closures are objects. So, are closures and functions the same? The answer is yes and no. The main difference between a UDF and a closure is that closures have access to the lexical environment in which they are declared. Both functions and closures can be manipulated at runtime and passed around to other functions and closures or returned from other functions and closures. Phew!

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
    };

    display();
}

hello();
```

If we execute this template via CommandBox, our output will be **luis**. This means the `display` closure has access to its surroundings to display the `name` variable. It can manipulate it, add to it, remove from it, and more.

## Returned Closures/High-Order Functions

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

In this case, the `makeAdder` creates a function that will add the passed-in variable with another via a delay of execution. You can then execute the resultant closures `add` with another number to get your calculation of `3` in this case.

**Funky!!**

## Passed Closures

CFML also has the concept of functional programming using several modern operations, like `map(), reduce(), filter(), each(), etc` you can pass closures into other functions for operating on different data structures.

```java
fruitArray = [
    { fruit='apple', rating=4 }, 
    { fruit='banana', rating=1 }, 
    { fruit='orange', rating=5 }, 
    { fruit='mango', rating=2 }, 
    { fruit='kiwi', rating=3 }
];

favoriteFruits = fruitArray.filter( function( item ){
     return item.rating >= 3;
} );
systemOutput( favoriteFruits );
```

Please note that you can construct your very own functional member functions on your objects and generate very functional custom DSL (Domain Specific Languages) by being creative.

## Delayed Execution

Another big advantage of leveraging closures for functional programming is that closures are the blueprint of a function and are not executed until you want to. They are useful for delaying execution and great for design patterns like observers, filters, iterators, and much more.

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

A closure retains a copy of variables visible at its creation. The global variables (like ColdFusion specific scopes) and the local variables (including declaring or outer function's local and arguments scope) are retained at the time of a closure creation. Functions are static.

The following details the scope of closure based on the way they are defined:

<table><thead><tr><th width="223">Scenario</th><th>Scope</th></tr></thead><tbody><tr><td>In a CFC function</td><td>Closure argument scope, enclosing function local scope and argument scope, this scope, variable scope, and super scope</td></tr><tr><td>In a CFM function</td><td>Closure argument scope, enclosing function local scope and argument scope, this scope, variable scope, and super scope</td></tr><tr><td>As function argument</td><td>Closure argument scope, variable scope, and this scope and super scope (if defined in CFC component).</td></tr></tbody></table>

In a closure, the following is the order of search for an unscoped variable:

* Closure's `local` scope
* Closure's `arguments` scope
* Outer function' `local` scope if available
* Owner function's `local` scope if available
* ColdFusion built-in scope

## isClosure()

CFML has a built-in function called `isClosure()` that allows you to evaluate if a variable is a closure or not:

```java
if( isClosure( arguments.body ) ){
    arguments.body();
}
```

## Lambda Expressions or Arrow Functions

Supported only in Lucee and Adobe 2018+.

{% hint style="danger" %}
Please note that they are not REAL lambdas or pure functions. Pure functions are not supposed to interact with their environment and should have no side effects on their surroundings. However, in ColdFusion, they are just implemented using the expression syntax, not the semantic nature of pure functions.
{% endhint %}

Arrow functions reduce much of the syntax around creating closures. In its simplest form, you can eliminate the `function` keywords, curly braces, and `return` statements. Arrow expressions implicitly return the results of the expression body.

```java
// Using a traditional closure
makeSix = function(){ return 5 + 1; }

// Using an arrow expression
makeSix = () => 5 + 1;

// returns 6
systemOutput( makeSix() );
```

A simple arrow expression with multiple arguments:

```java
// Takes two numeric values and adds them
add = ( numeric x, numeric y ) => x + y;

// returns 4
systemOutput( add( 1, 3 ) );
```

A complex arrow expression with an argument:

```java
// Takes a numeric value and returns a string
isOdd = ( numeric n ) => {
  if( n % 2 == 0 ){
    return 'even';
  } else {
    return 'odd';
  }
};

// returns 'odd'
SystemOutput( isOdd( 1 ) );

// returns 'even'
SystemOutput( isOdd( 10 ) );
```
