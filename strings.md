# Strings

In CFML strings are a type of variables that are used to store collections of letters and numbers.  Usually defined within single or double quotes ( `'` or `"` ).  Some simple strings would be `"hello"` or `"This sentence is a string!"`. Strings can be anything from `""`, the empty string, to really long sets of text.

Please note that the underlying type for a string in CFML is the Java String, which is immutable; meaning it can never change.  Thus, when concatenating strings together, a new string object is always created. This is a warning that if you will be doing many string concatenations, you will have to use a Java data type to accelerate the concatenations.  You have been warned.

## Character Extractions

In Lucee server you can actually reference characters in a string stream via their position in the string using array syntax: `varname[ position ]`.  Please note that string and array positions in CFML start at 1 and not 0.

```js
name = "luis";
writeoutput( name[1] ) => will produce l
```

## Common String Functions

You can find all the available string functions here: https://cfdocs.org/string-functions.  Below are some common ones that are handy to memorize:

### Len

* Call `len()` on a string to get back the number of characters in the string. For instance `Len( "Hello ")` would give you back **6** (notice the trailing space is counted). You can also use member functions: `a.len()`.

### Trim
* The`Trim` instruction removes leading and trailing spaces and control characters from a string. For instance `Trim("Hello ")` would give you back `Hello` (notice the trailing space is removed). Combine this with `Len` for example `Len( Trim( "Hello ") )` and you would get back `5`.  You can also use member functions:

```js
a.trim().len()
```

### Replace
* The `Replace` instruction replaces occurrences of **substring1** in a string with **substring2**, in a specified scope. The search is case sensitive and the scope default is one. For instance, `Replace("Hello", "e", "")` would give you back **Hllo** after replacing the _first occurrence of e_, or `Replace("Good Morning!", "o", "e", "All")` would give you **Geed Merning!** 

### RemoveChars 
* Call `RemoveChars` to remove characters from a string. For instance, `RemoveChars("hello bob", 2, 5)` would give you back **hbob**. 

### Mid
 
* The `mid` instruction extracts a substring from a string. For instance, I could call `Mid("Welcome to CFML Jumpstart",4,12)` and it would give you back: **come to CFML**.

### ListToArray

Another great function is `listToArray()` which can take any string and convert it to an array according to a delimiter.  The default delimiter is a comma `,`, but you can use any 1 or combination of characters.

```js
a = "luis,majano,lucas,alexia,veronica";
myArray = a.listToArray();
```

### Combining Strings and Variables

Combining and interpolating strings is part of any programming language and an integral part.  We can do both by building upon some language operators.

#### Combining strings 

If you have 2 or more strings, you can concatenate them by using the `&` operator:

```
name = "Luis";
a = "Hello " & name & " how are you today?"
```

