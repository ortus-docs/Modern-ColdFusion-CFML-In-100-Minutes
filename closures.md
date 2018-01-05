# Closures

> A closure is the combination of a function and the lexical environment within which that function was declared.

Remember that functions in CFML are objects, closures are objects as well.  So are closures and functions the same? The answer is yes and no.  The main difference between a UDF and a closure is that closures have access to their lexical environment in which they are declared.

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

## Delayed Execution

Another big advantage of leveraging closures for functional programming is that closures are the blueprint of a function and are not executed until you want to. Meaning they are useful for delaying execution and great for design patterns like: observer, filters, iterators and much more.

```java
var observe = function( val ){
    // manipulate the val and return it
    
    return val;
}
```