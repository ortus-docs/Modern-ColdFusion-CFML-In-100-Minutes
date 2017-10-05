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

Since CFML is a case-insensitive language, the structure defined above will store the keys all in uppercase.  If you want the exact casing to be preserved in the structure, then surround the keys with quotes (`"`).