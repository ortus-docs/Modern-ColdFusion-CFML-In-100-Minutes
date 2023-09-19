---
description: Collection of key-value pairs; a data dictionary
---

# Structures

A structure is a collection of data where each element of data is addressed by a **name or key** and it can hold a value of any type.  Like a dictionary but on steroids:

```javascript
// Create a struct via function
myStruct = structnew()

// Create a struct via literal syntax
myStruct = {}
```

{% hint style="success" %}
**Tip** Underneath the hood, all CFML structures are based on the `java.util.Map` interface. So if you come from a Java background, structures are untyped `HashMaps`.
{% endhint %}

As an analogy, think about a refrigerator. If we’re keeping track of the produce inside the fridge, we don’t really care about where the produce is in, or basically: **order doesn’t matter**. Instead, we organize things by name, which are unique, and each name can have any value. The name _grapes_ might have the value 2, then the name _lemons_ might have the value 1, and _eggplants_ the value 6.

{% hint style="info" %}
All CFML structures are passed to functions as memory references, not values. Keep that in mind when working with structures. There is also the `passby=reference|value` attribute to function arguments where you can decide whether to pass by reference or value.
{% endhint %}

## Key-Value Pairs

A structure is an _unordered collection_ where the data gets organized as a key and value pair. CFML syntax for structures follows the following syntax:

```javascript
produce = {
    grapes     = 2,
    lemons     = 1,
    eggplants  = 6
};
```

{% hint style="success" %}
**Tip** Please note that `=` sign and `:` are interchangeable in CFML. So you can use any to define your structures.
{% endhint %}

Since CFML is a case-insensitive language, the above structure will store all keys in uppercase. If you want the **exact casing** to be preserved in the structure, then surround the keys with quotes (`"`).

{% hint style="info" %}
The exact casing is extremely important if you will be converting these structures into JSON in the future.
{% endhint %}

```javascript
produce = {
    "grapes"     = 2,
    "lemons"     = 1,
    "eggplants"  = 6
};
```

![](<../assets/Screen Shot 2017-10-05 at 4.46.02 PM.png>)

The _key_ is the address, and the _value_ is the data at that address. Please note that the _value_ can be ANYTHING. It can be an array, an object, a simple value, or even an embedded structure. It doesn't matter.

## Retrieving Values

Retrieving values from structures can be done via dot or array notation or the `structFind()` function. Let's explore these approaches:

### Array Notation

```javascript
writeOutput( "I have #produce[ "grapes" ]# grapes in my fridge!" );
writeOutput( "I have #produce[ "eggplants" ]# eggplants in my fridge!" );
```

### Dot Notation

```javascript
writeOutput( "I have #produce.grapes# grapes in my fridge!" );
writeOutput( "I have #produce.eggplants# eggplants in my fridge!" );
```

### structFind( structure, key, \[defaultValue ] )

[https://cfdocs.org/structfind](https://cfdocs.org/structfind)

<pre class="language-javascript"><code class="lang-javascript"><strong>// Member Function
</strong><strong>writeOutput( "I have #produce.find( "grapes" )# grapes in my fridge!" );
</strong>writeOutput( "I have #produce.find( "eggplants" )# eggplants in my fridge!" );

// Global Function
writeOutput( "I have #structFind( produce, "grapes" )# grapes in my fridge!" );
writeOutput( "I have #structFind( produce, "eggplants" ) eggplants in my fridge!" );
</code></pre>

{% hint style="warning" %}
However, please be aware that when dealing with native Java hashmaps, we recommend using array notation as the case is preserved in array notation, while in dot notation, it does not.
{% endhint %}

{% hint style="success" %}
CFML offers also the `structGet()` function which will search for a key or a key path.  If there is no structure or array present in the path, this function creates structures or arrays to make it a valid variable path. [https://cfdocs.org/structget](https://cfdocs.org/structget)
{% endhint %}

### Safe Navigation

CFML also supports the concept of [safe navigation](operators.md#safe-navigation-operator) when dealing with structures.  Sometimes it can be problematic when using dot notation on nested structures since some keys might not exist or be `null`.  You can avoid this pain by using the safe navigation operator `?.` instead of the traditional `.` , and combine it with the elvis operator `?:` so if null, then returning a value.

```javascript
user = { age : 40 }

echo( user.age ) // 40
echo( user.salary ) // throws exception
echo( user?.salary ) // nothing, no exception
echo( user?.salary ?: 0 ) // 0
```

## Setting Values

I can also set new or override structure values a la carte.  You can do so via array/dot notation or via the `structInsert(), structUpdate()` functions ([https://cfdocs.org/structinsert](https://cfdocs.org/structinsert), [https://cfdocs.org/structupdate](https://cfdocs.org/structupdate))

```javascript
// new key using default uppercase notation
produce.apples = 3;
// new key using case sensitive key
produce[ "apples" ] = 3;

// I just ate one grape, let's reduce it
produce[ "grapes" ] = 1; // or
produce.grapes--;

produce.insert( "newVeggie", 2 )
produce.update( "newVeggie", 1 )

structInsert( produce, "carrots", 5 )
structUpdate( produce, "carrots", 2 )
```

{% hint style="success" %}
**Tip** You can use the `toString()` call on any structure to get a string representation of its keys+values: `produce.toString()`
{% endhint %}

## Checking Contents & Size

CFML also offers some useful methods when dealing with structures:

| Function          | Member Function |
| ----------------- | --------------- |
| `structIsEmpty()` | `isEmpty()`     |
| `structCount()`   | `count()`       |

## Key Values & Existence

Here are some great functions that deal with getting all key names, and key values or checking for existence:

| Function            | Member Function |
| ------------------- | --------------- |
| `structKeyArray()`  | `keyArray()`    |
| `structKeyList()`   | `keyList()`     |
| `structKeyExists()` | `keyExists()`   |

```javascript
produce.keyArray()
    .each( (item) => echo( item ) )
    
writeOutput( "My shopping bag has: #produce.keyList()# " )
writeOutput( "Do you have carrots? #produce.keyExists( 'carrots' )#" )
```

## Structure Types

In CFML, not only can you create case-insensitive unordered structures but also the following types using the `structNew()` function ([https://cfdocs.org/structnew](https://cfdocs.org/structnew))

<table><thead><tr><th width="349">Type</th><th width="129.33333333333331" data-type="checkbox">Adobe 2018</th><th width="135" data-type="checkbox">Adobe 2021</th><th data-type="checkbox">Lucee</th></tr></thead><tbody><tr><td><code>casesensitive</code></td><td>false</td><td>true</td><td>false</td></tr><tr><td><code>normal</code></td><td>true</td><td>true</td><td>true</td></tr><tr><td><code>ordered</code> or <code>linked</code></td><td>true</td><td>true</td><td>true</td></tr><tr><td><code>ordered-casesensitive</code></td><td>false</td><td>true</td><td>false</td></tr><tr><td><code>soft</code></td><td>false</td><td>false</td><td>true</td></tr><tr><td><code>synchronized</code></td><td>false</td><td>false</td><td>true</td></tr><tr><td><code>weak</code></td><td>false</td><td>false</td><td>true</td></tr></tbody></table>

Here is the signature for the `structnew()` function on Adobe engines:

```javascript
structNew( [type[[,sortType][,sortOrder][,localeSensitive]|[,callback]]] )
```

Here is the signature for Lucee engines ([https://docs.lucee.org/reference/functions/structnew.html](https://docs.lucee.org/reference/functions/structnew.html))

```
structNew( [type, [onMissingKey] ] )
```

Now let's create some different types of structures

```javascript
produce = structNew();
pickyProduce = structNew( "casesensitive" )

queue = structNew( "ordered" )
pickyQueue = structNew( "ordered-casesensitive" )
linkedList = structNew( 'ordered' );
cache = structnew( 'soft' );
```

### Literal Syntax

You can also use literal syntax for some of these types:

```javascript
// ordered struct
myStruct = [:] or [=]

// Case sensitive struct ACF Only
myStruct = ${}

// Case sensitive ordered struct ACF Only
myStruct = $[=]

```

## Common Methods

Once you create structures, you can use them in many funky ways. Please check out all the [structure functions](https://cfdocs.org/struct-functions) and all the structure modern [member functions](https://cfdocs.org/member) that are available to you.

![](<../assets/Screen Shot 2017-10-05 at 4.57.20 PM.png>)

As you can see, there are many cool methods for detecting keys, values, lengths, counts, etc. A very cool method is `keyArray()` which gives you the listing of keys as an array:

![](<../assets/Screen Shot 2017-10-05 at 4.58.09 PM.png>)

## Looping Over Structures

You can use different constructs for looping over structures:

* `for` loops
* `loop` constructs
* `each()` closures

```javascript
for( var key in produce ){
 systemOutput( "I just had #produce[ key ]# #key#" );
}

produce.each( function( key, value ){
  systemOutput( "I just had #value# #key#" );
} );
```

### Multi-Threaded Looping

As of now, only Lucee and Adobe 2021 allows you to leverage the `each()` operations in a multi-threaded fashion.  The `structEach()` or `each()` functions allow for a `parallel` and `maxThreads` arguments so the iteration can happen concurrently on as many `maxThreads` as supported by your JVM.

```java
structEach( struct, callback, parallel:boolean, maxThreads:numeric );
each( collection, callback, parallel:boolean, maxThreads:numeric );
```

This is incredibly awesome as now your callback will be called concurrently!  However, please note that once you enter concurrency land, you should shiver and tremble.  Thread concurrency will be of the utmost importance, and you must ensure that var scoping is done correctly and that appropriate locking strategies are in place.

```java
myStruct.each( function( key, value ){
   myservice.process( value );
}, true, 20 );
```

Even though this approach to multi-threaded looping is easy, it is not performant and/or flexible.  Under the hood, the engines use a single thread executor for each execution, do not allow you to deal with exceptions, and if an exception occurs in an element processor, good luck; you will never know about it.  This approach can be verbose and error-prone, but it's easy.  You also don't control where the processing thread runs and are at the mercy of the engine. &#x20;

### ColdBox Futures Parallel Programming

If you would like a functional and much more flexible approach to multi-threaded or parallel programming, consider using the ColdBox Futures approach (usable in ANY framework or non-framework code).  You can use it by installing ColdBox or WireBox into any CFML application and leveraging our `async` programming constructs, which behind the scenes, leverage the entire Java Concurrency and Completable Futures frameworks.

{% embed url="https://coldbox.ortusbooks.com/digging-deeper/promises-async-programming/parallel-computations" %}
ColdBox Futures and Async Programming
{% endembed %}

Here are some methods that will allow you to do parallel computations:

* `all( a1, a2, ... ):Future` : This method accepts an infinite amount of future objects,  closures, or an array of closures/futures to execute them in parallel.  When you call on it, it will return a future that will retrieve an array of the results of all the operations.
* `allApply( items, fn, executor ):array` : This function can accept an array of items or a struct of items of any type and apply a function to each of the items in parallel.  The `fn` argument receives the appropriate item and must return a result.  Consider this a parallel `map()` operation.
* `anyOf( a1, a2, ... ):Future` : This method accepts an infinite amount of future objects, closures, or an array of closures/futures and will execute them in parallel. However, instead of returning all of the results in an array like `all()`, this method will return the future that executes the fastest! Race Baby!
* `withTimeout( timeout, timeUnit )` : Apply a timeout to `all()` or `allApply()` operations.  The `timeUnit` can be days, hours, microseconds, milliseconds, minutes, nanoseconds, and seconds. The default is milliseconds.
