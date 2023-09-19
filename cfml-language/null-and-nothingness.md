---
description: null does mean something!
---

# Null & Nothingness

What is nothingness? Is there nothingness only in outer space? If a tree falls in the forest and nobody listens, does it make a sound? Starting to see the point? Does nothing really mean nothing? To be or not to be? OK, I think we are going on a philosophical tangent, so let's get back to our geekiness:

`null` is Java's way to refer to "nothingness.", something that does not exist and has no value. Support for the`null`keyword itself was introduced as an option in ColdFusion 2018 (as discussed in [this blog post](https://coldfusion.adobe.com/2018/07/null-support-in-coldfusion-2018/)) and has existed as an option in Lucee (as discussed in [this guide](https://docs.lucee.org/guides/cookbooks/NullSupport.html)).

## Full-Null Support

Please note that full null support is **NOT** the default in the CFML engines.  Meaning you will not be able to use the `null` keyword until it is activated or get real `null` values from databases or external services.  In reality, you still could simulate `null` without full null support in both engines, and sometimes you get an empty string, sometimes a full Java `null`.  So basically, the nonfull null support is a partial null support, which makes it hard for developers.  **So as a rule of thumb, we always recommend checking for nullness no matter WHAT!**

Eventually, this flag should default to true, in our opinion, and offer full-null support out of the box.&#x20;

Ok, back to activating full-null support.  You can do this in the admin or programmatically via the `Application.cfc` file, which can be used when building web applications. You can learn more [about it here](../beyond-the-100/applicationcfc.md)

{% code title="Application.cfc" %}
```java
component{
    this.nullSupport = true;
}
```
{% endcode %}

## Checking For Nullness

Use the `isNull()` or `isDefined()` methods to evaluate for nothingness.

```javascript
r = getMaybeData()
if( isNull( r ) ){
  // do something because r doesn't exist
}

if( isDefined( "r" ) ){

}
```

Also, remember that you can use the [Elvis operator](operators.md#elvis-operator-null-coalescing) to test for null and an operator and expression.

```javascript
results = getMaybeData() ?: "default value"
```

{% hint style="info" %}
We would recommend that you use `isNull()` as it expresses coherently its purpose. Since `isDefined()` can also evaluate expressions.
{% endhint %}

## Creating Nulls

You can create nulls in different ways in CFML. Let's explore these:

<table><thead><tr><th width="268">Approach</th><th width="98.33333333333331" data-type="checkbox">Full Null</th><th>Description</th></tr></thead><tbody><tr><td><code>null</code> keyword</td><td>true</td><td><code>r = null</code></td></tr><tr><td>Non returning function call</td><td>false</td><td>If a function returns nothing, its assignment will produce a null.<br><code>function getNull(){}</code><br><code>r = getNull()</code></td></tr><tr><td><code>nullValue()</code></td><td>false</td><td>Lucee only function.<br><code>r = nullValue()</code></td></tr><tr><td><code>javaCast( "null", "" )</code></td><td>false</td><td><p>Available in all engines</p><p><code>r = javaCast( "null", "" )</code></p></td></tr></tbody></table>

## In Practice

If you have three eggs, and eat three eggs, then you might think you have "nothing," but in terms of eggs you have "0". Zero is something, it’s a number, and it’s not nothing.

If you’re working with words and have a string like "hello" then delete the "h", "e", "l"s, and "o" you might think you’d end up with nothing, but you really have "" which is an empty string. It’s still something.

Null in CFML is usually encountered when you ask for something that doesn’t exist. When looking at arrays, for instance, we created a list with five elements then asked CFML to give us the sixth element of that list. There is no sixth element, so CFML gave us null. It isn’t that there’s a blank in that sixth spot (""), it’s not a number 0, it’s nothingness – null.

**Examples**

```java
function getData( filter ){

    if( isNull( arguments.filter ) ){
      // then do this
    } else {
      // use the filter
    }

}

function returnsNull(){
  if( key.exists( "invalid" ) ){
    return key[ "invalid" ];
  }
}

results = returnsNull();

writeOutput( isNull( results ) );
```

Also note that if a function returns **nothing** it will be the same as returning `null`.
