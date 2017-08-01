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

You can find all the available string functions here: https://cfdocs.org/string-functions.

### Len
* Call `Len` on a string to get back the number of characters in the string. For instance `Len("Hello ")` would give you back **6** (notice the trailing space is counted).

### Trim
* The`Trim` instruction removes leading and trailing spaces and control characters from a string. For instance `Trim("Hello ")` would give you back **Hello** (notice the trailing space is removed). Combine this with `Len` for example `Len(Trim("Hello "))` and you would get back **5**.

### Replace
* The `Replace` instruction replaces occurrences of **substring1** in a string with **substring2**, in a specified scope. The search is case sensitive and the scope default is one. For instance, `Replace("Hello", "e", "")` would give you back **Hllo** after replacing the _first occurrence of e_, or `Replace("Good Morning!", "o", "e", "All")` would give you **Geed Merning!** 

### RemoveChars 
* Call `RemoveChars` to remove characters from a string. For instance, `RemoveChars("hello bob", 2, 5)` would give you back **hbob**. 

### Mid
 
* The `mid` instruction extracts a substring from a string. For instance, I could call `Mid("Welcome to CFML Jumpstart",4,12)` and it would give you back: **come to CFML**.

Experiment with the following samples in a CFML file.



`<cfscript>`
```javascript
tester = "Good Morning Everyone!";

writeOutput ("#len(tester)#<br/>");
writeOutput (Replace (tester, "o", "e", "All") & "<br/>");
writeOutput (RemoveChars (tester, 2, 5) & "<br/>");

t2 = "sample,data,from,a,CSV";
t3 = Mid (t2,8,len(t2));

writeOutput (t3 & "<br/>");
```
`</cfscript>`

Often a string may store a list like the *t2* variable in the last example. A string for storing a list isn't the best for performance and usage. Using an array for a list is so much better. We can convert a list into an *array* using *ListToArray*. We'll discuss arrays in an upcoming section. Try out these next examples in the CFML file assuming we have the code from the last example:


`<cfscript>`
```javascript
t4 = ListToArray(t3);
writeOutput(t4[2]);
```
`</cfscript>`

The numbers inside the "[]" brackets specify which item of the array you want pulled out. They're numbered starting with 1. So the first example pulls out the "2" array item. This "t4" array contains position "1", the beginning of the list, up to position "4", the ending of the array.

### Combining Strings and Variables

It is extremely common that we want to combine the value of a variable with other strings. For instance, lets start with this example string:

**Happy Saturday!**

When we put that into the CFML file, it just spits back the same string. If we were writing a proper program we might want it to greet the user when they start the program by saying **Happy** then the day of the week. So we can't just put a string like **Happy Saturday!** or it'd be saying Saturday even on Tuesday.

What we need to do is combine a variable with the string. There are two ways to do that. The first approach is called *string concatenation* which is basically just adding strings together:


`<cfscript>`
```js
today   = "Saturday";
message = "Happy " & today & "!";

writeOutput( message );
```
`</cfscript>`

In the first line we setup a variable to hold the day of the week. Then we printed the string *Happy* combined with the value of the variable "today" and the string *!*. You might be thinking, "What was the point of that since we still wrote *Saturday* in the first line?" Ok, well, if you were writing a real program you'd use CFMLs built-in date instructions like this:

```js
today = DayOfWeek(Now());
```

`Now()` gets the current date and time of the computer running the ColdFusion server. "DayOfWeek" returns an integer in the range 1 (Sunday) to 7 (Saturday) for the day of the week. We still don't have the day of week as string. Try this:

#### Tag Syntax

```cfm
<cfset today = DayOfWeekAsString(DayOfWeek(Now())) />
<cfset message = "Happy " & today & "!" />
<cfoutput>
#message#
</cfoutput>
```

#### Script Syntax

`<cfscript>`
```javascript
today = DayOfWeekAsString(DayOfWeek(Now()));
message = "Happy " & today & "!";
writeOutput(message);
```
`</cfscript>`

Great, no errors and our output looks correct. "DayOfWeekAsString" did the trick. There is another string combination called *string interpolation*.

**String interpolation** is the process of sticking data into the middle of strings. We use the symbols `#` around the "variable" where in a string the value should be inserted. Inside those hashes we can put any variable and output it in that spot. Our previous example "message" could be rewritten like this:

#### Tag Syntax

```cfm
<cfset message = "Happy #today#!" />
```

#### Script Syntax

```javascript
message = "Happy #today#!";
```

If you compare the output you'll see the second example gives the exact same results. The code itself is a little more compact and, personally, I find it much easier to read.

Basically *interpolating* means evaluate the code inside this `#` wrapper and put it into the string.

***

[Previous Components](components) --- [Next Numbers](numbers)