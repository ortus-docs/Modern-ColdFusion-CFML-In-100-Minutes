# Comments

Comments are necessary and essential for any programming language. CFML is no different with helping you add code comments in both script and tag syntax.

## Tag Comments

You can use the `<!---` and `--->` Syntax to comment within a CFML template. This is very similar to HTML comments but adding an extra `-` to demarcate it as a CFML comment.

```markup
HTML Comment
<!-- I am an HTML Comment -->

ColdFusion Comment
<!---  I am a ColdFusion Comment --->
```

## Script Comments

If you are within a CFC or in a `<cfscript>` block you can use an alternate style for comments. You can leverage `//` for single line comments and the following for multi-line comments:

```javascript
/*
  Multi 
  Line
  Comments
  are
  great!
*/

// Single line comment
```

## Script "Javadoc" style comments

A multi-line block can affect the metadata of a `component` or `function` if the opening line contains 2 asterisks. Also, for readability, some people will start each line of the comment with an asterisk. The CF engines will parse out those starting asterisks and they will not appear in the component or the function metadata.

```javascript
    /**
    * This is the hint for the function
    *
    * @param1 This is the hint for the param
    */
    function myFunc( string param1 ){
  }
```

## CFDoc Style Comments

In the CFML world you can write [JavaDoc](http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html) comments in what we call **CFDoc** comments. We leverage the [DocBox](https://github.com/Ortus-Solutions/DocBox) library to generate documentation according to object metadata and comments.

```javascript
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

You can see some examples of advanced CFC documentation here: [http://apidocs.ortussolutions.com.s3.amazonaws.com/coldbox/5.0.0/index.html](http://apidocs.ortussolutions.com.s3.amazonaws.com/coldbox/5.0.0/index.html)

