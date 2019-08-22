# Variables

In CFML, variables are just pointers to a piece of data. They can hold **any** value you like and even change it's value or type at runtime. In some languages, you need to specify the type of data you want your variable to hold. In CFML, you do not need to assign one as everything is dynamic. The Lucee server even goes further and infers types according to the initial value you assign to your variable.

```javascript
a = "string"; // string
b = now(); // data
c = 123; // integer
d = 1.34; // float
f = false; // boolean
```

Please note that assignments are evaluated from right to left instead of traditional reading which is from left to right.

Open up the CommandBox Shell and go into **REPL** mode by typing `repl`. Every time you assign a value to a variable, the CommandBox REPL will output or echo the variable for you. Please note that in REPL mode the termination for a line of code is omitted. A line terminator in ColdFusion is the `;`.

![](../.gitbook/assets/variables.png)

As you can see, we can create strings, numerics, arrays, structs and so much more. No need for types or special assignments.

{% hint style="success" %}
The CommandBox REPL is based on a Lucee 5 server, that is why semi-colons are optional and the default syntax is script and not tags.
{% endhint %}

## Case Insensitive

CFML is a case-insensitive language. Meaning if you create a variable `a` and reference it as `A` they are the same. This can be a very big gotcha for developers coming from languages like Java or JavaScript. However, as best practice, we would recommend to **ALWAYS** use the same case as when you define the variable:

```javascript
a = "Hola Luis";
writeOutput( A );
```

## Naming Requirements

Most CFML variables have a few requirements imposed by the Virtual Machine \(VM\)

* Must begin with a letter, underscore, or Unicode currency symbol.
* Can contain any number of letters, numbers, underscore characters, and Unicode currency symbols.
* NO SPACES!
* Not case-sensitive

## Flexible Typing

You can also create a variable with one type and then switch it to another dynamically:

![](../.gitbook/assets/flexible-typing.png)

As you can see, the last equality wins! In this case, `a` is now an array.

## Outputting Variables

You can also output or evaluate variables by using the `#` operators and using the variable name:

```javascript
a = "Hola Luis";
writeoutput( "Welcome to CFML: #a#" );
```

Also note that using the `#` hashes for output on assignments can be reduntant:

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

CFML offers one of the most used functions/tags ever: `<cfdump>, writeDump()` and `<cfabort>, abort;`. These are used to dump the entire contents of a variable to the browser, console or even a file. You can then leverage the `abort` construct to abort the request and see the output of your dumped variables. This will work with both simple and complex variables. However, be very careful when using it with Nested ORM objects as you can potentially dump your entire database and crash the server. Leverage the `top` argument to limit the dumping.

```javascript
writeDump( complex );abort;

<cfdump var="#server#" abort=true>

writeDump( var=arrayOfORM, top=5 );abort;
```

### Server Debugging Templates

CFML Engines also allow you to turn on/off a debugging template that shows up at the bottom of requests when running in server mode. You can activate this debugging by logging in to the appropriate engine administrator and look for the **debugging** section. Turn it on and debug like a champ.

{% hint style="danger" %}
**Important:** Adobe Engines have a very evil setting called _Report Execution Times_, make sure it is always turned **OFF**. If you use it with any application that leverages Components, it will slow down your application tremendously.
{% endhint %}

## Paraming Variables

CFML allows you to set default values for variables in case you use a variable and it doesn't exist. You can use the `<cfparam>` tag or the `param` construct:

```markup
<cfparam name="myVariable" default="luis">
```

or

```javascript
param myVariable = "luis"
```

{% hint style="success" %}
You can even assign types to parameterize variables and much more. Check out the docs for it: [https://cfdocs.org/cfparam](https://cfdocs.org/cfparam)
{% endhint %}

## Checking For Existence

You can verify if variables exist in many different ways. The next section showcases how variables are stored in visibility and persistence scopes which are all structures or hash maps in Java terms. Meaning you can leverage structure operations for checking for existence and much more. Below are several ways to verify variable existence:

* `isDefined()` - Evaluates a string value to determine whether the variable

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
    writeOutput( myVariable );
} else {
    writeOutput( "Not Defined!" );
}

// What is this variables scopes???
if( structKeyExists( variables, "myVariable" ) ){
    writeOutput( myVariable );
} else {
    writeOutput( "Not Defined!" );
}
```

## Java Integration

As we have discussed, CFML is a dynamic language built on-top of Java. Thus each variable internally is represented by a native Java data type: `String, Int, Float, Array, Vector, HashMap, etc`. This is important because each variable you create has member functions available to you that delegate or reflect back to its native Java class.

```javascript
a = "hello";
writeOutput( a.getClass().getName() );
```

If you run the script above in the REPL tool, you will see the output as `java.lang.String`. Therefore, the variable is typed as a String and can call on any method that `java.lang.String` implements. You can try this for the many types in CFML like structs, arrays, objects, etc.

### CFML Member Functions

A part from the native Java member functions available to you, CFML also allows you to call on each variable's data type functions and chain them to create nice language DSLs. This way you do not have to be passing variables into functions, but instead treat the variables as objects. You can see all the member functions available according to data type here: [https://cfdocs.org/member](https://cfdocs.org/member)

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

At [Ortus Solutions](https://www.ortussolutions.com) we have developed a set of development standards for many languages. You can find our [ColdFusion standards](https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md) here: [https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md](https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md)

