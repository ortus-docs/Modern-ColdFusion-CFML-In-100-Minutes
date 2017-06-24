# Syntax

There are two ways to write ColdFusion (CFML) code: in **tags** or in **script** syntax.  Modern CFML will dictate that your view or presentation layers will utilize the **tag** syntax in `cfm` files and the model or business layers will be all done in **script** syntax in `cfc` files. (MVC comes later)

* CFScript Syntax Guide - https://cfdocs.org/script

## Syntax Files
CFML includes a set of instructions you use in pages or components (classes). You will write one or more instructions in a file (`.cfm,.cfc`) then run the file through a CFML engine or Command Line Interpreter like CommandBox.

 * `cfm` - ColdFusion markup file, tag syntax is the default
 * `cfc` - ColdFusion Component file (Class), script syntax is the default. 
 
## Implicit Behavior

CFML also gives you a pre-set of defined [tags](https://cfdocs.org/tags) and [functions](https://cfdocs.org/functions) available to you in any type of file you write your code in.  These tags and functions allows you to extend the typical language constructs with many modern capabilities from database interaction to PDF generation.  

> **Hint** Please note that the CFML built-in functions are also `first-class` functions so they can be passed around as arguments to other functions or closures.


## Exploring Behavior

Three CFML instructions we will use in this tutorial are `cfset`, `cfoutput`, and `cfdump`. `cfset` is used to create a variable and assign it a value. Also `cfset` is used to call methods. `cfoutput` displays a variable's value. `cfdump` is used to display the contents of simple and complex variables, objects, components, user-defined functions, and other elements.

We might have a file named _myprogram.cfm_ and _Sample.cfc_ like this:

### Tag Syntax

**myprogram.cfm**

```html
<cfset s = new Sample() />
<cfoutput>#s.hello()#</cfoutput>
```

**Sample.cfc**

```xml
<cfcomponent output="false">
    <cffunction name = "hello" output="false">
        <cfreturn "Hello, World!" />
    </cffunction>
</cfcomponent>
```

### Script Syntax

**myprogram.cfm**

```js
<cfscript>
    s = New Sample();
    writeOutput(s.hello());
</cfscript>
```

**Sample.cfc**

```js
component{
    
    function hello(){
       return( "Hello, World!" );
    }
    
}
```

For the script example, _myprogram.cfm_ would have beginning/closing `<cfscript>` tags around the instructions, however, the script-based _Sample.cfc_ does not require `<cfscript>` tags around the instructions.

### PHP Syntax

#### myprogram.php

```php
<?php
    require("Sample.php");
    $s = new Sample();
    echo $s->hello();
?>
```

#### Sample.php

```php
<?php
class Sample
{
    public function hello() {
        return "Hello, World!";
    }
}
?>
```

### Ruby Syntax

#### myprogram.rb

```rb
class Sample
    def hello
        "Hello, World!"
end

s = Sample.new
puts s.hello
```

### Java Syntax


#### MyProgram.java

```java
public class MyProgram {

	public String hello(){
		return "Hello, world!";
	}
	
	public static void main(String[] args) {
		System.out.println( new MyProgram().hello() );
	}

}
```

## Coding Standards

At [Ortus Solutions](https://www.ortussolutions.com) we have developed a set of development standards for many languages.  You can find our [ColdFusion standards](https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md) here: https://github.com/Ortus-Solutions/coding-standards/blob/master/coldfusion.md