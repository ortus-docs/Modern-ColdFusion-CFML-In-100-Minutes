# JSON

CFML supports native JSON support via several key functions and some member functions.

## Core Functions

The core functions that deal with JSON are:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><code>deserializeJson( </code>
        </p>
        <p><code>  json, </code>
        </p>
        <p><code>  strictMapping, </code>
        </p>
        <p><code>  useCustomSerializer</code>
        </p>
        <p><code>)</code>
        </p>
      </td>
      <td style="text-align:left">Converts a JSON (JavaScript Object Notation) string data representation
        into CFML data, such as a struct or array. <a href="https://cfdocs.org/deserializejson">https://cfdocs.org/deserializejson</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>isJson( var )</code>
      </td>
      <td style="text-align:left">Evaluates whether a string is in valid JSON (JavaScript Object Notation)
        data interchange format. <a href="https://cfdocs.org/isjson">https://cfdocs.org/isjson</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><code>serializeJson( </code>
        </p>
        <p><code>  data, </code>
        </p>
        <p><code>  serializeQueryByColumns, </code>
        </p>
        <p><code>  useSecureJSONPrefix,</code>
        </p>
        <p><code>  useCustomSerializer </code>
        </p>
        <p><code>)</code>
        </p>
      </td>
      <td style="text-align:left">Converts CFML data into a JSON (JavaScript Object Notation) representation
        of the data. <a href="https://cfdocs.org/serializejson">https://cfdocs.org/serializejson</a>
      </td>
    </tr>
  </tbody>
</table>Here are some examples for you:

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

