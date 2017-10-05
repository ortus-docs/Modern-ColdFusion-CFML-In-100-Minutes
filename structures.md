# Structures

A structure is a collection of data where each element of data is addressed by a **name**. 

> **Tip** Underneath the hood, all CFML structures are based on the `java.util.Map` interface.  So if you are coming from a Java background, structures are just HashMaps. 

As an analogy, think about a refrigerator. If we’re keeping track of the produce inside the fridge, we don’t really care about where in the fridge the produce is in or basically: **order doesn’t matter**. Instead we organize things by name, which are unique, and each name can have any value. Like the name *grapes* might have the value 2, then the name *lemons* might have the value 1, and *eggplants* the value 6.

## Key-Value Pairs

A structure is an *unordered collection* where the data gets organized as a key and value pair.  CFML syntax for structures follows the following syntax:

```js
produce = {
    grapes     = 2,
    lemons     = 1,
    eggplants  = 6
};
```

> **Tip** Please note that `=` sign and `:` are interchangeable in CFML.  So you can use any to define your structures.

Since CFML is a case-insensitive language, the structure defined above will store the keys all in uppercase.  If you want the exact casing to be preserved in the structure, then surround the keys with quotes (`"`).

```js
produce = {
    "grapes"     = 2,
    "lemons"     = 1,
    "eggplants"  = 6
};
```

![](/assets/Screen Shot 2017-10-05 at 4.46.02 PM.png)


The *key* is used as the address and the *value* is the data at that address.  Please note that the *value* can be ANYTHING. It can be an array, an object, a simple value or even an embedded structure. It doesn't matter.

Retreiving values are also simple, just reference the structure by the key name:

```js
writeOutput( "I have #produce[ "grapes" ]# grapes in my fridge!" );
writeOutput( "I have #produce[ "eggplants" ]# eggplants in my fridge!" );
```

I can also set new or override values of the structure a-la-carte:

```js
// new key using default uppercase notation
produce.apples = 3;
// new key using case sensitive key
produce[ "apples" ] = 3;

// I just ate one grape, let's reduce it
produce[ "grapes" ] = 1; // or
produce.grapes--
```

## StructNew() Types

You can also declare new structures via the `structNew()` function.  This just basically assigns a struct to a variable. You will then be responsible for filling that structure out with data.

```js
produce = {};
produce = structnew();
```

But why would I ever use the function if I can do the `{}` notation which looks nicer?  The answer is that in CFML you can create different types of structures:

* `linked/ordered` - a struct with ordered keys that maintain insertion order
* `normal` - an unordered structure
* `soft` - a struct with Java soft referenced values, which can be cleared by the garbage collector if memory is needed.
* `weak` - a struct with Java weak referenced values, which do not prevent their referents from being garbage collected.

```js
linkedList = structNew( 'ordered' );
cache = structnew( 'soft' );
```

## Common Methods

Once you create structures you can use them in many funky ways.  Please check out all the [structure functions](https://cfdocs.org/struct-functions) and all the structure modern [member functions](https://cfdocs.org/member) that are available to you.

![](/assets/Screen Shot 2017-10-05 at 4.57.20 PM.png)

As you can see, there are many cool methods from detecting keys, values, length, counts, etc. A very cool method is `keyArray()` which gives you the listing of keys as an array:

![](/assets/Screen Shot 2017-10-05 at 4.58.09 PM.png)