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


## Naming Coding Standards

At [Ortus Solutions](https://www.ortussolutions.com) we have developed a set of development standards for many languages. You can find our [ColdFusion standards](https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md) here: https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md





