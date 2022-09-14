# Asynchronous Programming

We already covered basic threading in our core CFML section.  In this section we will cover the usage of asynchronous programming via futures and the `runAsync()` function built-in to Adobe ColdFusion 2018+ and Lucee 5.3+.

> A Future is an eventual result of an asynchronous operation.

{% embed url="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Future.html" %}
Java Future JDK (Useful Reference)
{% endembed %}

{% embed url="https://helpx.adobe.com/coldfusion/using/asynchronous-programming.html" %}
Adobe Guide on RunAsync()
{% endembed %}

## runAsync()

This function returns a **Future** object, which is an eventual result of an asynchronous operation.  Basically a representation of what the operation will produce, well, in the future.  It takes in two arguments and returns a CFML Future object.

* `callback:function` - Closure/Lambda function that returns a result to be resolved
* `timeout:numeric` - Timeout for the asynchronous process in milliseconds

```java
future = runAsync( function(){
	return "Hello World!";
} );
writeOutput( future.get() );

future = runAsync(function(){
       return 5;
} ).then( function( input ){
       return input + 2;
} );
result = future.get( 3 ); // 3 is timeout(in ms)
writeOutput(result);
```

## The Future Object

The return of the `runAsync()` function is a CFML Future, not a Java Future.  The Future Object has the following functions available:

| Function                     | ReturnType | Description                                                                                                                                                         |
| ---------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `cancel()`                   | Boolean    | Cancel the operation                                                                                                                                                |
| `complete( value )`          | value      | Sets a value to return from the future, usually from an empty future operation.                                                                                     |
| `error( callback, timeout )` | Future     | Register an error callback that will be called if the async operation fails or the timeout is reached                                                               |
| `error( callback )`          | Future     | Register an error callback that will be called if the async operation fails                                                                                         |
| `get()`                      | Any        | The value of the async operation once it finalizes.  **Caution: This operation blocks until the async operation finalizes.**                                        |
| `get( timeout )`             | Any        | The value of the async operation with a timeout in milliseconds.  **Caution: This operation blocks until the async operation finalizes.**                           |
| `isCancelled()`              | Boolean    | Verifies if the operation has been cancelled or not                                                                                                                 |
| `isDone()`                   | Boolean    | Verifies if the operation has finalized or not                                                                                                                      |
| `then( callback )`           | Future     | Once the first callback operation has finalized, call this secondary callback with the value of the previous operation and return another Future                    |
| `then( callback, timeout )`  | Future     | Once the first callback operation has finalized, call this secondary callback with the value of the previous operation and return another Future but with a timeout |

{% hint style="info" %}
All timeouts are in milliseconds
{% endhint %}

With the future you can now create fluent functional programming constructs to deal with your async operation.  You can create different error listeners and even continue processing the value the async operation produced in another asyncronous operation.  Much like a pipeline of never ending futures!

```java
future = runAsync( function(){
  return 5;
}).then( function(input){
  return input + 2;
}).error( function(){
  return "Error occurred.";
});
result = future.get();
writeOutput(result);
```

You can mix and match the callback functions to create a nice asyncronous pipeline.  Just note that if you call the `get()` operation immediately that will BLOCK the execution until the async operation finalizes, which kinda defeats the purpose of the async operation.  If you do not want to block, then use the `then()` approach, where that callback will be called for you with the result of the async operation and then you can do your post-processing.  The alternative is to sit and poll the `isDone()` or `isCancelled()` operations, and YUCK!
