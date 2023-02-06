---
description: Strings in CFML/Java are immutable! Remember that well!
---

# Strings

In CFML, strings are a type of variable that is used to store collections of letters and numbers. Usually defined within single or double quotes ( `'` or `"` ). Some simple strings would be `"hello"` or `"This sentence is a string!"`. Strings can be anything from `""`, the empty string, to really long sets of text.

Please note that the underlying type for a string in CFML is the Java [String](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html), which is immutable, meaning it can never change. Thus, a new string object is always created when concatenating strings together. This is a warning that if you do many string concatenations, you will have to use a Java data type to accelerate the concatenations ([String Builders](https://www.baeldung.com/java-string-builder-string-buffer)). You have been warned.

{% hint style="info" %}
More on String Builders: [https://www.baeldung.com/java-string-builder-string-buffer](https://www.baeldung.com/java-string-builder-string-buffer)
{% endhint %}

## Character Extractions

In Adobe 2021+ and Lucee server, you can reference characters in a string stream via their position in the string using array syntax: `varname[ position ]`. Please note that string and array positions in CFML start at 1 and not 0.

```javascript
name = "luis";
writeoutput( name[ 1 ] ) => will produce l
```

Adobe has taken this further, and you can use negative indices to get characters from the end backward:

```javascript
name = "luis";
writeoutput( name[ -1 ] ) => will produce s
```

## Character Extractions by Range

Adobe 2018+ also supports extraction as ranges using the following array syntax:

```javascript
array[ start:stop:step ]
```

Which is extremely useful for doing character extractions in ranges

```javascript
 data = "Hello CFML. You Rock!";
 
 writeOutput( data[ 1 ] ) // Returns H
 writeOutput( data[ -3 ] ) // Returns c
 writeOutput( data[ 4:10:2 ] ) // Returns l FL
 writeOutput( data[ 4:12 ] ) // Returns lo CFML
 writeOutput( data[ -10:-4:2]) // Returns o o
```

## Common String Functions

You can find all the available string functions here: [https://cfdocs.org/string-functions](https://cfdocs.org/string-functions). Below are some common ones that are handy to memorize:

### Len

* Call `len()` on a string to get back the number of characters in the string. For instance `Len( "Hello ")` would give you back **6** (notice the trailing space is counted). You can also use member functions: `a.len()`.

### Trim

* The`Trim` instruction removes leading and trailing spaces and controls characters from a string. For instance, `Trim("Hello ")` would give you back `Hello` (notice the trailing space is removed). Combine this with `Len` for example `Len( Trim( "Hello ") )` and you would get back `5`.  You can also use member functions:

```javascript
a.trim().len()
```

### Replace

* The `Replace` instruction replaces occurrences of **substring1** in a string with **substring2**, in a specified scope. The search is case-sensitive and the scoped default is one. For instance, `Replace("Hello", "l", "")` would give you back **Helo** after replacing the _first occurrence of l_, or `Replace("Good Morning!", "o", "e", "All")` would give you **Geed Merning!**&#x20;

### RemoveChars

* Call `RemoveChars` to remove characters from a string. For instance, `RemoveChars("hello bob", 2, 5)` would give you back **hbob**.&#x20;

### Mid

* The `mid` instruction extracts a substring from a string. For instance, I could call `Mid("Welcome to CFML Jumpstart", 4, 12)` and it would give you back: **come to CFML**.

### ListToArray

Another great function is `listToArray()` which can take any string and convert it to an array according to a delimiter. The default delimiter is a comma `,`, but you can use any one or a combination of characters.

```javascript
a = "luis,majano,lucas,alexia,veronica";
myArray = a.listToArray();
```

## Combining Strings and Variables

Combining and interpolating strings is part of any programming language and an integral part. We can do both by building upon some language operators.

### Combining strings

If you have 2 or more strings, you can concatenate them by using the `&` operator:

```javascript
name = "Luis";
a = "Hello " & name & " how are you today?";
```

You can also concatenate and assign using the `&=` operator.  Please [check out the operators](operators.md#assignment-operators) section for more on string assignment operators.

## Interpolating Strings

Interpolating is where we stick a string within another string. In CFML, we use the `#` hashes to output a variable to the stream in context. This means we can interpolate into any string:

```javascript
name = "luis";
welcome = "Good morning #name#, how are you today?";
writeoutput( welcome );
```

That's it! If you surround any **simple** variable with `#` hashes, CFML will interpret the variable. Now try this with a complex variable and see what happens:

```javascript
complex = [1,2,3];
welcome = "Good morning #complex#, how are you today?";
writeoutput( welcome );
```

