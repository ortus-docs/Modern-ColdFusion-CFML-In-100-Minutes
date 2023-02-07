---
description: JSON all things!
---

# JSON

CFML supports native JSON support via several key functions and some member functions.

## Serialize

CFML gives us the `serializeJSON()` function to convert any piece of data to its JSON representation ([https://cfdocs.org/serializejson](https://cfdocs.org/serializejson))

```javascript
serializeJson(
 var
 [, serializeQueryByColumns = false ]
 [, useSecureJSONPrefix = false ]
 [, useCustomSerializer = false ]
)
```

Pass in any complex or simple variable to the `var` argument and JSON will be produced:

```javascript
person = { name = "Luis Majano", company = "Ortus Solutions", year = 2006};
writeOutput( serializeJSON( person ) );
```

If you are in Lucee, you can even use the `toJSON()` member function:

```javascript
person = { name = "Luis Majano", company = "Ortus Solutions", year = 2006};
writeOutput( person.toJSON() );
```

### Key Casing

By default CFML will convert the keys in a struct to uppercase in the result JSON document:

```javascript
person = { name = "Luis Majano", company = "Ortus Solutions", year = 2006};
writeOutput( serializeJSON( person ) );

// Will become
{ "NAME" : "Luis Majano", "COMPANY" : "Ortus Solutions", "YEAR" : 2006 }
```

If you want to preserve the key casing then wrap them in double/single quotes and define the case:

```javascript
person = { 
    'Name' = "Luis Majano", 
    'company' = "Ortus Solutions", 
    'year' = 2006
};

// Will become
{ "Name" : "Luis Majano", "company" : "Ortus Solutions", "year" : 2006 }
```

### Possible Casting Issues

Adobe ColdFusion may incorrectly serialize some strings if they can be automatically converted into other types, like numbers or booleans. One workaround is to use a CFC with [cfproperty](https://cfdocs.org/cfproperty) to specify types. Another workaround is to prepend `Chr(2)` to the value and it will be forced to a string, however, that is an unofficial/undocumented workaround.  A more formal workaround is to  call `setMetadata()` as a member function on a `struct` to force a type:

```javascript
myStruct = { "zip"="00123" };
myStruct.setMetadata( { "zip": "string" } );
writeOutput( serializeJSON(myStruct) );
```

## Deserialize

The inverse of serialization is deserialization ([https://cfdocs.org/deserializejson](https://cfdocs.org/deserializejson)).  CFML gives you the `deserializeJSON()` function that will take a JSON document and produce native CFML data structures for you.

```javascript
deserializeJSON(
 json
 [, strictMapping = true ]
 [, useCustomSerializer = false ]
)
```

Just pass a JSON document, and off we go with native structs/arrays/dates/strings and booleans.

```java
if( isJson( mydata ) ){
    return deserializeJSON( data );
}

person = deserializeJSON( '{"company":"Ortus","name":"Mr OrtusMan"}' );
writeOutput( person.company );
```

This function can also be used as a member function in any string literal:

```java
var deserializedData = myjsonString.deserializeJson();
var data = '[]'.deserializeJson();
```

## Is this JSON?

CFML has a function to test if the incoming string is valid JSON ([https://cfdocs.org/isjson](https://cfdocs.org/isjson)) or not: `isJSON()`

```javascript
isJSON( "[ 1, 2, 3 ]" )
```

