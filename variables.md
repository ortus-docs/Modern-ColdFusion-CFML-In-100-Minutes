# Variables

In CFML, variables are just pointers to a piece of data.  They can hold **any** value you like and even change it's value or type at runtime.  In some languages, you need to specify the type of data you want your variable to hold.  In CFML, you do not need to assign one as everything is dynamic.  The Lucee server even goes further and infers types according to the initial value you assign to your variable.

```js
a = "string"; // string
b = now(); // data
c = 123; // integer
d = 1.34; // float
f = false; // boolean
```

Please note that assignments are evaluated from right to left instead of traditional reading which is from left to right.

Open up the CommandBox Shell and go into **REPL** mode by typing `repl`.  Every time you assign a value to a variable, the CommandBox REPL will output or echo the variable for you. Please note that in REPL mode the termination for a line of code is omitted.  A line terminator in ColdFusion is the `;`.

![](/assets/variables.png)

As you can see, we can create strings, numerics, arrays, structs and so much more.  No need for types or special assignments.

> **Hint** The CommandBox REPL is based on a Lucee 4.5 server, that is why semi-colons are optional and the default syntax is script and not tags.

## Case Insensitive

CFML is a case-insensitive language.  Meaning if you create a variable `a` and reference it as `A` they are the same.  This can be a very big gotcha for developers coming from languages like Java or JavaScript.  However, as best practice, we would recommend to **ALWAYS** use the same case as when you define the variable:

```js
a = "Hola Luis";
writeOutput( A );
```

## Naming Requirements

Most CFML variables have a few requirements imposed by the VM

* Must begin with a letter, underscore, or Unicode currency symbol.
* Can contain any number of letters, numbers, underscore characters, and Unicode currency symbols.
* NO SPACES!
* Not case-sensitive


## Flexible Typing

You can also create a variable with one type and then switch it to another dynamically:

![](/assets/flexible-typing.png)

As you can see, the last equality wins! In this case, `a` is now an array.

## Outputting Variables

You can also output or evaluate variables by using the `#` operators and using the variable name:

```js
a = "Hola Luis";
writeoutput( "Welcome to CFML: #a#" );
```

Also note that using the `#` hashes for output on assignments can be reduntant:

**Dont' do this**
```js
a = "hello luis";
b = #a#;
or 
b = "#a#";
```

**Do this**
```js
a = "hello luis";
b = a;
```


## Paraming Variables

CFML allows you to set default values for variables in case you use a variable and it doesn't exist.  You can use the `<cfparam>` tag or the `param` construct:

```xml
<cfparam name="myVariable" default="luis">
```

or 

```js
param myVariable = "luis"
```

> **Hint** You can even assign types to param variables and much more. Check out the docs for it: [https://cfdocs.org/cfparam](https://cfdocs.org/cfparam)

## Variable Scopes

In the CFML language, there are many persistence and visibility scopes that exist for variables to be placed in.  These are differentiated by context: in a CFC, in a function, tag, thread or in a template.  All CFML scopes are implemented as structures of key-value name pairs. The default scope for variable storage is called `variables`.  Thus you can refer variables like this:

```js
a = hello
writeOutput( a )
or 
writeOutput( variables.a )
```

We will study these scopes further, but here are the major scopes delineated by context:

### Persistence Scopes

Can be used in any context, used for persisting variables for a period of time.

* session - stored in server RAM or external storages tracked by unique web visitor
* client - stored in cookies, databases, or external storages (simple values only)
* application - stored in server RAM or external storage tracked by the running ColdFusion application
* cookie - stored in a visitor's browser
* server - stored in server RAM for ANY application for that CFML instance
* request - stored in RAM for a specific user request ONLY
* cgi - read only scope provided by the servlet container and CFML

### Template Scopes

* variables - The default scope where all variables are assigned to.

### Component Scopes

* variables - Private scope, visible internally to the CFC
* this - Public scope, visible from the outside world
* static - No need for a CFC instance, available as a CFC representation

### Function Scopes
* variables - Has access to private variables within a Component or Page
* this - Has access to public variables within a Component or Page
* local - Function scoped variables, only exist within the function execution. Referred to as `var` scoping
* arguments - Incoming variables to a function

### Tag Scopes
* attributes - Incoming tag attributes
* variables - The default scope for variable assignments

### Thread Scopes
* attributes - Passed variables via a thread
* thread - A thread specific scope that can be used for storage and retrieval
* local - Variables local to the thread context

## Naming Coding Standards

At [Ortus Solutions](https://www.ortussolutions.com) we have developed a set of development standards for many languages. You can find our [ColdFusion standards](https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md) here: https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md





