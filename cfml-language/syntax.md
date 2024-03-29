---
description: Script or Tags? Choose wisely!
---

# Syntax

There are two ways to write CFML code: in **tags** or in **script** syntax. Modern CFML will dictate that your view or presentation layers will utilize the **tag** syntax in `cfm` files, and the model or business layers will all be done in **script** syntax in `cfc` files. (MVC comes later).  There are no differences in functionality between them; it's pure syntax.

* CFScript Syntax Guide - [https://cfdocs.org/script](https://cfdocs.org/script)

## Syntax Files

CFML includes a set of instructions you use on pages (`.cfm`) or components (classes -`cfc`). You will write one or more instructions in a file (`.cfm,.cfc`) then run the file through a CFML engine or Command Line Interpreter like CommandBox.

* `cfm` - ColdFusion markup file, tag syntax is the default and used for views
* `cfc` - The default is the ColdFusion Component file (Class or Object), script syntax.&#x20;

## Implicit Behavior

CFML also gives you a pre-set of defined [tags](https://cfdocs.org/tags) and [functions](https://cfdocs.org/functions) available to you in any file you write your code in. These tags and functions allow you to extend the typical language constructs with many modern capabilities, from database interaction to PDF generation.  They are basically automatic imports.

{% hint style="success" %}
**Tip:** Please note that the CFML built-in functions are also **first-class functions** so that they can be passed around as arguments to other functions or closures or saved as variables.
{% endhint %}

## Exploring Behavior

Three CFML instructions we will use in this section are `cfset`, `cfoutput`, and `cfdump`.

* `cfset` is used to create a variable and assign it a value.
* `cfoutput` displays a variable's value to the output stream.
* `cfdump` is used to display the contents of simple and complex variables, objects, components, user-defined functions, and other elements to the output stream.

We might have a file named _myprogram.cfm_ and _Sample.cfc_ like this:

### Tag Syntax

{% tabs %}
{% tab title="myprogram.cfm" %}
```markup
<cfset s = new Sample()>
<cfoutput>#s.hello()#</cfoutput>
```
{% endtab %}
{% endtabs %}

### Script Syntax

{% tabs %}
{% tab title="myprogram.cfm" %}
```java
<cfscript>
    s = new Sample();
    writeOutput( s.hello() );
</cfscript>
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Tip:** Please note that if you want to write in script in a tag-based file, you must use an opening and closing `<cfscript>` tag.
{% endhint %}

{% tabs %}
{% tab title="Sample.cfc" %}
```javascript
component{

    function hello(){
       return "Hello, World!";
    }

}
```
{% endtab %}
{% endtabs %}

Please note that no types and not even any visibility scopes you might be used to are present. CFML can also infer variable types on more distinct variables like dates, booleans, or numbers. However, please note that you can fully leverage types if you like:

{% tabs %}
{% tab title="Sample.cfc" %}
```javascript
component{

    public string function hello(){
       return "Hello, World!";
    }

}
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
By default, the return type of every function and/or argument is **any**. Thus, it can be determined at runtime as a dynamic variable.
{% endhint %}

### Semi-Colons

Please note that semi-colons are used to demarcate line endings in CFML `;`. However, the Lucee Server engine and Adobe ColdFusion 2018+ treat semi-colons as optional, while Adobe ColdFusion 2016 or below does not.  Also, note the CommandBox REPL does NOT require semi-colons.

### Tags In Script

Lucee and Adobe ColdFusion 11+ will allow you to write your CFML tags in script syntax. You basically eliminate the starting `<` and ending `>` enclosures and create a block by using the `{` and `}` mustaches.

```javascript
cfhttp(method="GET", charset="utf-8", url="https://www.google.com/", result="result") {
    cfhttpparam(name="q", type="formfield", value="cfml");
}
```

## Polyglot References

As we now live in a world of polyglot developers, we have added references below to other languages to see the differences and similarities between CFML and other major languages in usage today. Please note that this section is merely academic and to help developers from other language backgrounds to understand the intricacies of the ColdFusion (CFML) syntax.

### PHP Syntax

{% tabs %}
{% tab title="myprogram.php" %}
```php
<?php
    require("Sample.php");
    $s = new Sample();
    echo $s->hello();
?>
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Sample.php" %}
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
{% endtab %}
{% endtabs %}

### Ruby Syntax

{% tabs %}
{% tab title="myprogram.rb" %}
```ruby
class Sample
    def hello
        "Hello, World!"
    end
end

s = Sample.new
puts s.hello
```
{% endtab %}
{% endtabs %}

### Java Syntax

{% tabs %}
{% tab title="MyProgram.java" %}
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
{% endtab %}
{% endtabs %}

## Coding Standards

At [Ortus Solutions](https://www.ortussolutions.com), we have developed a set of development standards for many languages. You can find our standards here: [https://github.com/Ortus-Solutions/coding-standards](https://github.com/Ortus-Solutions/coding-standards).
