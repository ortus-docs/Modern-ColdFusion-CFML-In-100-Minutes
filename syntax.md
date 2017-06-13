# Syntax

There are two ways to write CFML code: in tags or in script syntax.  Modern CFML will dictate that your view or presentation layers will utilize the tag syntax and the model or business layers will be all done in script syntax.

CFML includes a set of instructions you use in pages. You will write one or more instructions in a file then run the file through a CFML engine or Command Line Interpreter like CommandBox. Three CFML instructions we will use in this tutorial are `cfset`, `cfoutput`, and `cfdump`. `cfset` is used to create a variable and assign it a value. Also `cfset` is used to call methods. `cfoutput` displays a variable's value. `CFDUMP` is used to display the contents of simple and complex variables, objects, components, user-defined functions, and other elements.

We might have a file named _myprogram.cfm_ and _Sample.cfc_ like this:

### Tag Syntax

#### myprogram.cfm

```cfm
<cfset s = New Sample() />
<cfoutput>#s.hello()#</cfoutput>
```

#### Sample.cfc

```cfm
<cfcomponent>
    <cffunction name = "hello">
        <cfreturn "Hello, World!" />
    </cffunction>
</cfcomponent>
```

### Script Syntax

#### myprogram.cfm

```cfm
<cfscript>
    s = New Sample();
    writeOutput(s.hello());
</cfscript>
```

#### Sample.cfc

```javascript
component {
    public string function hello() {
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

Next section [Variables](variables)