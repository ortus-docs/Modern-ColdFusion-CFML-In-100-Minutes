---
description: name = "Amazing Programmer"
---

# Variables

In CFML, variables are just pointers to a piece of data. They can hold **any** value you like and even change their value or **type** at runtime. In some languages, you need to specify the type of data you want your variable to hold at compile-time. You do not need to assign one in CFML, as everything is dynamic or inferred. The Lucee and Adobe 2021 servers infer types according to the initial value you assign to your variable.

```javascript
a = "string"; // string
b = now(); // datetime
c = 123; // integer
d = 1.34; // float
f = false; // boolean, or a string for Adobe :)
```

{% hint style="danger" %}
Please note that assignments are evaluated from right to left instead of traditional reading from left to right.
{% endhint %}

Open up the CommandBox Shell and go into CommandBox **REPL** mode by typing `repl`. Every time you assign a value to a variable, the CommandBox REPL will output or echo the variable for you. Please note that in REPL mode, the termination for a line of code is omitted. A line terminator in ColdFusion is the `;`.

![](<../assets/variables (1).png>)

As you can see, we can create [strings](strings.md), [numerics](numbers.md), [arrays](arrays.md), [structs](structures.md), and so much more. No need for types or special assignments.  The ColdFusion engine will determine or infer it and use it accordingly, thus a dynamic language.

{% hint style="success" %}
The CommandBox REPL is based on a Lucee 5 server, which is why semi-colons are optional, and the default syntax is script and not tags.
{% endhint %}

## Case Insensitive

**CFML is a case-insensitive language** as well. Meaning if you create a variable `a` and reference it as `A` they are the same. This can be a big gotcha for developers from languages like Java or JavaScript. However, as best practice, we would recommend **ALWAYS** using the same case as when you define the variable:

**Don't do this**

```javascript
a = "Hola Luis";
writeOutput( A );
```

**Do this**

```javascript
a = "Hola Luis";
writeOutput( a );
```

## Naming Requirements

Most CFML variables have a few requirements imposed by the Virtual Machine (VM)

* It must begin with a letter, underscore, or Unicode currency symbol.
* It can contain letters, numbers, underscore characters, and Unicode currency symbols.
* NO SPACES!
* Not case-sensitive

#### Reserved Words

As with any programming language, there are specific names you just can't use, and some you can use, but they can be very confusing for developers or the engine.  Here is a list of some of those:

* The name of any of the internal ColdFusion engine scopes: `form, session, cgi, client, url, application, function`
  * Technically you can create the variable by long scoping (`local.form`), but it is confusing and error-prone.  So please be careful.



<table><thead><tr><th width="372">Reserved Word</th><th width="176.33333333333331" align="center">Lucee</th><th align="center">Adobe</th></tr></thead><tbody><tr><td><code>abstract</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>and</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>break</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>case</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>catch</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>continue</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>contains</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>default</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>do</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>else</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>eq</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>eqv</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>false</code></td><td align="center">√</td><td align="center">√</td></tr><tr><td><code>finally</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>final</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>for</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>function</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>gt</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>gte</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>import</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>imp</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>in</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>is</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>if</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>interface</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>lt</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>lte</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>local</code> (within a function)</td><td align="center"></td><td align="center">√</td></tr><tr><td><code>neq</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>not</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>null</code> (If null support is on)</td><td align="center">√</td><td align="center">√</td></tr><tr><td><code>pageenconding</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>or</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>return</code></td><td align="center"></td><td align="center">v</td></tr><tr><td><code>switch</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>true</code></td><td align="center">√</td><td align="center">√</td></tr><tr><td><code>try</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>while</code></td><td align="center"></td><td align="center">√</td></tr><tr><td><code>xor</code></td><td align="center"></td><td align="center">√</td></tr></tbody></table>

## Flexible Typing

You can also create a variable with one type and then switch it to another dynamically:

![](<../assets/flexible-typing (1) (1).png>)

As you can see, the last equality wins! In this case, `a` is now an array.

## Types

As we are now seeing, CFML is a typeless language, but internal types always exist.  CFML will automatically cast so you can do flexible typing assignments when evaluating expressions.  It does all the tedious and hard job for you.  If we were to categorize CFML variables into categories, these would be:

| Category    | Description                                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------- |
| **Binary**  | Raw data from files such as images, pdfs, etc                                                                       |
| **Complex** | A data container that represents more than one value: structures, arrays, queries, XML document objects, etc.       |
| **Objects** | Complex constructs representing data and functional operations.  ColdFusion Components or Java Objects.             |
| **Simple**  | One value and used directly in expressions. These include numbers, strings, floats, booleans, and date-time values. |

CFML also includes many validation functions available to you to test for the type of variable you are working with.  You can also use the `getmetdata()` function to get the metadata about the variable as well.

```javascript
qData = getMetadata( query )
a = now()
writedump( a.getMetadata() )
```

* `isArray()`
* `isBinary()`
* `isBoolean()`
* `isCustomFunction()`
* `isClosure()`
* `isDate()`
* `isDateObject()`
* `isDDX()`
* `isJSON()`
* `isNumeric()`
* `isNumericDate()`
* `isObject()`
* `isNull()`
* `isPDFFile()`
* `isPDFObject()`
* `isQuery()`
* `isSimpleValue()`
* `isSpreadsheetFile()`
* `isSpreadsheetObject()`
* `isStruct()`
* `isWDDX()`
* `isXML()`
* `isXmlDoc()`



### Conversions

You can also in CFML convert variables from one type to another.  Here are some functions that will assist you in conversions:

* `arrayToList()`
* `binaryDecode()`
* `binaryEncode()`
* `charsetDecode()`
* `charsetEncode()`
* `deserializeJSON()`
* `entityToQuery()`
* `hash()`
* `hmac()`
* `HTMLParse()`
* `lcase()`
* `listToArray()`
* `parseNumber()`
* `serializeJSON()`
* `toBase64()`
* `toBinary()`
* `toScript()`
* `toString()`
* `URLDecode()`
* `URLEncode()`
* `URLEncodedFormat()`
* `val()`
* `XMLFormat()`
* `XMLParse()`
* `XMLTransform()`

{% hint style="info" %}
Please note that some of them can be used as member functions directly on a specific object type. [https://cfdocs.org/conversion%2Dfunctions](https://cfdocs.org/conversion-functions)
{% endhint %}



## Outputting Variables (Interpolation)

You can also output or evaluate variables by using the `#` operators and using the variable name. This is referred to as interpolation in some languages:

```javascript
a = "Hola Luis"
writeoutput( "Welcome to CFML: #a#" )
// Echo is the same as writeOutput but LUCEE only
echo( "Welcome" )
```

Also, note that using the `#` hashes for output on assignments can be redundant if you do NOT use string interpolation but just variable assignments.

**Don't do this**

```javascript
a = "hello luis";
b = #a#;
or 
b = "#a#";
```

**Do this**

```javascript
a = "hello luis";
b = a;
```

## Debugging Variables

CFML offers one of the most used functions/tags ever: `<cfdump>, writeDump()` and `<cfabort>, abort;`. These are used to dump the entire contents of a variable to the browser, console, or even a file. You can then leverage the `abort` construct to abort the request and see the output of your dumped variables. This will work with both simple and complex variables. However, be very careful when using it with Nested ORM objects, as you can potentially dump your entire database and crash the server. Leverage the `top` argument to limit dumping.

```javascript
writeDump( complex );abort;

<cfdump var="#server#" abort=true>

writeDump( var=arrayOfORM, top=5 );abort;
```

### Server Debugging Templates

CFML Engines also allow you to turn on/off a debugging template that shows up at the bottom of requests when running in server mode. You can activate this debugging by logging in to the appropriate engine administrator and looking for the **debugging** section. Turn it on and debug like a champ.

{% hint style="danger" %}
**Important:** Adobe Engines have a very evil setting called _Report Execution Times_, make sure it is always turned **OFF**. If you use it with any application that leverages Components, it will slow down your application tremendously.
{% endhint %}

## Paraming Variables

CFML allows you to set default values for variables in case you use a variable that doesn't exist. You can use the `<cfparam>` tag or the `param` construct:

```markup
<cfparam name="myVariable" default="luis">
```

or

```javascript
param myVariable = "luis";
```

{% hint style="success" %}
You can even assign types to parameterize variables and much more. Check out the docs for it: [https://cfdocs.org/cfparam](https://cfdocs.org/cfparam)
{% endhint %}

## Checking For Existence

You can verify if variables exist in many different ways. The following section showcases how variables are stored in visibility and persistence scopes which are all structures or hash maps in Java terms. Meaning you can leverage structure operations for checking for existence and much more. Below are several ways to verify variable existence:

*   `isDefined()` - Evaluates a string value to determine whether the variable

    named in it exists.
* `isNull()` - Returns `true` if the specified object is null, else `false`.
* `structKeyExists( key, value )` - Verifies if the specified key variable exists in a structure.

```javascript
// Notice the variable name is in quotes
if( isDefined( "myVariable" ) ){
    writeOutput( myVariable );
} else {
    writeOutput( "Not Defined!" );
}

// Notice that the variable is NOT in quotes
if( isNull( myVariable ) ){
    writeOutput( "Not Defined!" );
} else {
    writeOutput( myVariable );
}

// What is this variables scopes???
if( structKeyExists( variables, "myVariable" ) ){
    writeOutput( myVariable );
} else {
    writeOutput( "Not Defined!" );
}
```

## Java Integration

As we have discussed, CFML is a dynamic language built on Java. Thus each variable internally is represented by a native Java data type: `String, Int, Float, Array, Vector, HashMap, etc`. This is important because each variable you create has member functions available to you that delegate or reflect its native Java class.

```javascript
a = "hello";
writeOutput( a.getClass().getName() );
```

If you run the script above in the REPL tool, you will see the output as `java.lang.String`. Therefore, the variable is typed as a `String` and can call on any method that `java.lang.String` implements. You can try this for the many types in CFML, like structs, arrays, objects, etc.

## Member Functions

Besides the native Java member functions available to you, CFML also allows you to call on each variable's data type functions and chain them to create friendly language DSLs. This way, you do not have to pass variables into functions but treat the variables as objects. You can see all the member functions available according to data type here: [https://cfdocs.org/member](https://cfdocs.org/member)

Here are some examples:

```javascript
// Function passing
var myArray = [];
ArrayAppend( myArray, "objec_new" );
ArraySort( myArray, "ASC" );

// Member Functions
myArray.append( "objec_new" );
myArray.sort( "ASC" );

// Java Functions + CFML Functions
var myProductObject = createObject( "java", "myJavaclass" );
myjavaList = myProductObject.getProductList();
myjavaList.add( "newProduct" ); // Java API

myjavaList.append( "newProduct" ); // CF API
myjavaList.sort( "ASC" );

// DSL Chaining
s="the";
s = s.listAppend("quick brown fox", " ")
     .listAppend("jumps over the lazy dog", " ")
     .ucase()
     .reverse();
```

#### Member functions for the following data types are supported:

* Array
* String
* List
* Struct
* Date
* Spreadsheet
* XML
* Query
* Image

Please see [https://cfdocs.org/member](https://cfdocs.org/member) for further information on member functions.

## Naming Coding Standards

At [Ortus Solutions](https://www.ortussolutions.com), we have developed a set of development standards for many languages. You can find our ColdFusion standards here: [https://github.com/Ortus-Solutions/coding-standards](https://github.com/Ortus-Solutions/coding-standards)
