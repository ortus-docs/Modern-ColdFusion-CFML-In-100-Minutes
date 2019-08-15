# Comments

Comments are necessary and essential for any programming language. CFML is no different with helping you add code comments in both script and tag syntax.

## Tag Comments

You can use the `<!---` and `--->` Syntax to comment within a CFML template \(`.cfm`\). This is very similar to HTML comments but adding an extra `-` to demarcate it as a CFML comment.

```markup
HTML Comment
<!-- I am an HTML Comment -->

ColdFusion Comment
<!---  I am a ColdFusion Comment --->
```

## Script Comments

If you are within a CFC or in a `<cfscript>` block you can use an alternate style for comments. You can leverage `//` for single line comments and the following for multi-line comments:

```java

/**
 * Constructor, called by your Application CFC
 *
 * @COLDBOX_CONFIG_FILE The override location of the config file
 * @COLDBOX_APP_ROOT_PATH The location of the app on disk
 * @COLDBOX_APP_KEY The key used in application scope for this application
 * @COLDBOX_APP_MAPPING The application mapping override, only used for Flex/SOAP apps, this is auto-calculated
 * @COLDBOX_FAIL_FAST By default if an app is reiniting and a request hits it, we will fail fast with a message. This can be a boolean indicator or a closure.
 */


/**
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

## CFCDoc Style Comments

In the CFML world you can write [JavaDoc](http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html) comments in what we call **CFCDoc** comments. We leverage the [DocBox](https://github.com/Ortus-Solutions/DocBox) library to generate documentation according to object metadata and comments.

{% code-tabs %}
{% code-tabs-item title="MyAwesome.cfc" %}
```java
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
     * @vars The vars I need
     * @vars.generic Array
     *
     * @return MyComponent
     * @throws SomethingException
     */
    function init( required wirebox, required vars ){
        variables.wirebox = arguments.wirebox;
        return this;
    }


}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

You can see some examples of advanced CFC documentation here: [http://apidocs.ortussolutions.com.s3.amazonaws.com/coldbox/5.0.0/index.html](http://apidocs.ortussolutions.com.s3.amazonaws.com/coldbox/5.0.0/index.html)

{% hint style="success" %}
**Tip**: VSCode has some great plugins for generating this type of documentation on your CFCs.  We recommend the following extensions:

* **Align** - Helps align everything
* **AutoCloseTag** - Helps close comment and well all tags
* **DocumentThis** - Automatically generates detailed JSDoc, CFCDoc comments in TypeScript and JavaScript files.
{% endhint %}

