# Comments

Comments are necessary and essential for any programming language.  CFML is no different with helping you add code comments in both script and tag syntax.


## Tag Comments

You can use the `<!---` and `--->` Syntax to comment within a CFML template. This is very similar to HTML comments but adding an extra `-` to demarcate it as a CFML comment.


```html

HTML Comment
<!-- I am an HTML Comment-->

ColdFusion Comment
<!---  I am a ColdFusion Comment--->

```

## Script Comments

If you are within a CFC or in a `<cfscript>` block you can use an alternate style for comments.  You can leverage `//` for single line comments and the following for multi-line comments:

```js
/**
* Multi 
* Line
* Comments
* are
* great!
*/

// Single line comment
```

## CFDoc Style Comments

In the CFML world you can write [JavaDoc](http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html) comments in what we call **CFDoc** comments.  We leverage the [DocBox](https://github.com/Ortus-Solutions/DocBox) library to generate documentation according to object metadata and comments.

```js
/**
* This is my component
* 
* @author Luis Majano
*/
component extends="Base" implements="IHello" singleton{

    /**
    * The Settings
    */
    property name="settings";


    /**
    * Constructor
    *
    * @wirebox The Injector
    * @wirebox.inject wirebox
    */
    function init( required wirebox ){
        variables.wirebox = arguments.wirebox;
        return this;
    }


}
```

