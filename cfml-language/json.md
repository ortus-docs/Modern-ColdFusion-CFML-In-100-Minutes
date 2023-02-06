---
description: JSON all things!
---

# JSON

CFML supports native JSON support via several key functions and some member functions.

## Core Functions

The core functions that deal with JSON are:

| Function | Description |
| -------- | ----------- |

| <p><code>deserializeJson(</code></p><p> <code>json</code></p><p> <code>[ , strictMapping ]</code></p><p> <code>[ , useCustomSerializer ]</code></p><p><code>)</code></p> | Converts a JSON (JavaScript Object Notation) string data representation into CFML data, such as a struct or array. Only the 'json' argument is required. [https://cfdocs.org/deserializejson](https://cfdocs.org/deserializejson) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

| `isJson( var )` | Evaluates whether a string is in valid JSON (JavaScript Object Notation) data interchange format. [https://cfdocs.org/isjson](https://cfdocs.org/isjson) |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |

| <p><code>serializeJson(</code></p><p> <code>data</code></p><p> <code>[ , serializeQueryByColumns ]</code></p><p> <code>[ , useSecureJSONPrefix ]</code></p><p> <code>[ , useCustomSerializer ]</code></p><p><code>)</code></p> | Converts CFML data into a JSON (JavaScript Object Notation) representation of the data. Only the 'data' argument is required. [https://cfdocs.org/serializejson](https://cfdocs.org/serializejson) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

```java
if( isJson( mydata ) ){
    return deserializeJSON( data );
}

serializeJSON( myQuery, true );
serializeJSON( myData );

person = deserializeJSON( '{"company":"Ortus","name":"Mr OrtusMan"}' );
writeOutput( person.company );
```

## Member Functions

You can call the `deserializeJSON()` from any string literal:

```java
var deserializedData = myjsonString.deserializeJson();
var data = '[]'.deserializeJson();
```
