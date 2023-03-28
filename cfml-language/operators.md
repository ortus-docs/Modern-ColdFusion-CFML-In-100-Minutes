---
description: Operate all things++--==!^%/\
---

# Operators

**Operators** are the foundation of **any** programming language. Operators are symbols that help a programmer to perform specific mathematical, structuring, destructuring, and logical computations on operands (variables or expressions).  We can categorize the CFML operators into the following categories:

1. Arithmetic/Mathematical
2. Assignment
3. Logical
4. Comparison
5. Ternary
6. Elvis (Null Coalescing)
7. Function
8. Collections



{% hint style="info" %}
You will see that CFML does not have native [bitwise](https://en.wikipedia.org/wiki/Bitwise\_operation) operators, but it does implement bitwise operations via functions since functions can also be operators in CFML: `bitAnd, bitMaskClear, bitMaskRead, bitMaskSet, bitNot, bitOr, bitSHLN, bitSHRN, bitXOR` . You can find much more information here: [https://cfdocs.org/math%2Dfunctions](https://cfdocs.org/math-functions)
{% endhint %}

{% hint style="success" %}
For more information about bitwise operations, you can read more here: [https://en.wikipedia.org/wiki/Bitwise\_operation](https://en.wikipedia.org/wiki/Bitwise\_operation)
{% endhint %}

{% hint style="danger" %}
CFML does not offer the capability to overload operators like other languages.
{% endhint %}

## Operator Precedence

The order of precedence exists in CFML, just like in mathematics.  You can also control the order of precedence by using the grouping operator `()` like in mathematics, the magical ordering parenthesis.

{% code lineNumbers="true" %}
```javascript
^
*, /
\
MOD
+, -
&
EQ, NEQ, LT, LTE, GT, GTE, CONTAINS, DOES NOT CONTAIN, ==, !=, >, >=, <, <=
NOT, !
AND, &&
OR, ||
XOR
EQV
IMP
```
{% endcode %}

{% hint style="success" %}
Remember that using parenthesis `(Grouping Operator)` is very important to denote precedence.
{% endhint %}

## Arithmetic Operators

These operators are used to perform arithmetic/mathematical operations on operands.

| Operator | Name                | Description                                                                                                                                                                                     |
| -------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `+`      | Add                 | `a = 1 + 4`                                                                                                                                                                                     |
| `-`      | Subtract            | `a = 4 - 2`                                                                                                                                                                                     |
| `*`      | Multiply            | `a = 4 * b`                                                                                                                                                                                     |
| `/`      | Divide              | `a = 4 / myVariable`                                                                                                                                                                            |
| `^`      | Exponentiate        | `a = 2^2 // 4`                                                                                                                                                                                  |
| `%, MOD` | Modulus / Remainder | `5 % 2 = 1` or `5 mod 2`                                                                                                                                                                        |
| `\`      | Integer Divide      | `a = 7 \ 3` is 2. Please note it does not round off the integer.                                                                                                                                |
| `++`     | Increment           | <p><code>a = b++</code> assign b to a and THEN increment b<br><code>a = ++b</code> increment b and THEN assign to a</p>                                                                         |
| `--`     | Decrement           | <p><code>a = b--</code> assign b to a and THEN decrement b<br><code>a = --b</code> decrement b and THEN assign to a</p>                                                                         |
| `-`      | Negate              | `a = -b` Negate the value of b                                                                                                                                                                  |
| `+`      | Positive            | `a = +b` Make the value of b a positive number                                                                                                                                                  |
| `()`     | Grouping            | <p>The grouping operator is used just like in mathematics, to give precedence to operations.<br><code>result = 3 * (2+3)</code> which is not the same as<br><code>result = 3 * 2 + 3</code></p> |

## Assignment Operators

These operators are usually used for compound evaluations and assignments.

| Operator | Name                   | Description                                                                                                                                                                                                           |
| -------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `=`      | Assignment             | <p><code>a = 5</code> The way to assign a value to a variable. You can also assign them to multiple variables by chaining them:<br><code>a=b=c=5</code> which is the same as saying: <br><code>a=5;b=5;c=5</code></p> |
| `+=`     | Compound Add           | `a += b` is equivalent to `a = a + b`                                                                                                                                                                                 |
| `-+`     | Compound Substract     | `a -= b` is equivalent to `a = a - b`                                                                                                                                                                                 |
| `*=`     | Compound Multiply      | `a *= b` is equivalent to `a = a * b`                                                                                                                                                                                 |
| `/+`     | Compound Divide        | `a /= b` is equivalent to `a = a / b`                                                                                                                                                                                 |
| `%=`     | Compound Modulus       | `a %= b` is equivalent to `a = a % b`                                                                                                                                                                                 |
| `&=`     | Compound Concatenation | <p>A way to concatenate strings together<br><code>a = "hello "</code><br><code>a &#x26;= "luis"</code> The result will be <code>hello luis</code></p>                                                                 |
| `&`      | Concatenation          | Concatenates two strings: `"Hola" & space & "Luis"`                                                                                                                                                                   |

###

## Logical Operators

Logical operators perform logic between values or values, usually denoting a `boolean` result.

| Operator   | Name         | Description                                                                                                                                                                                                                                                       |
| ---------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `!,NOT`    | Negation     | `!true = false` or `a = not true`                                                                                                                                                                                                                                 |
| `&&,AND`   | And          | <p>Returns true if both operands are true.<br><code>a = b &#x26;&#x26; c</code></p>                                                                                                                                                                               |
| `\|\|, OR` | Or           | <p>Returns true if either operand is true.<br><code>a = b || c</code></p>                                                                                                                                                                                         |
| `XOR`      | Exclusive Or | <p>Returns true when either of the operands is true (one is true, and the other is false), but both are not true, and both are not false.<br><code>true XOR true = false</code><br><code>true XOR false = true</code><br><code>false XOR false = false</code></p> |
| `EQV`      | Equivalence  | <p>The exact opposite of an exclusive or. Meaning that it will return true when both operands are either true or false.<br><code>true EQV true = true</code><br><code>true EQV false = false</code><br><code>false EQV false = true</code></p>                    |
| `IMP`      | Implication  | A implies B is equivalent to `if a then b`. A imp b is false ONLY if a is true and b is false; else, it returns true always.                                                                                                                                      |

###

## Comparison Operators

Comparison operators are used when comparing two values, expressions, or variables.  The return of a comparison is either `true` or `false`.

| Operator                                                              | Name                 | Description                                                                                                                               |
| --------------------------------------------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `eq,==`                                                               | Equality             | True if `a eq b` or `a == b`                                                                                                              |
| <p><code>neq,</code> <br><code>!=,</code><br><code>&#x3C;></code></p> | Not Equal            | The opposite of equality: `a neq b, a != b, a <> b`                                                                                       |
| `===`                                                                 | Identity             | <p>Returns true if the operands are equal in value and in type.<br><code>2 === "2" // false</code><br><code>2 === 2   // true</code> </p> |
| `!===`                                                                | Negated Identity     | Same as the identity operator but negating the result.                                                                                    |
| `gt,>`                                                                | Greater than         | If the left operand is greater in value than the right operand                                                                            |
| `gte, >=`                                                             | Greater than o equal | If the left operand is greater than or equal in value than the right operand                                                              |
| `lt, <`                                                               | Less than            | If the left operand is less than in value than the right operand                                                                          |
| `lte, <=`                                                             | Less than or equal   | If the left operand is less than or equal in value than the right operand                                                                 |
| `contains,ct`                                                         | Contains             | <p>Returns true if the left operand contains the right one.<br><code>'hello' contains 'lo'</code></p>                                     |
| `does not contain, nct`                                               | Negated contains     | <p>Returns true if the left operand does NOT contain the right one.<br><code>'hello' does not contain 'pio'</code></p>                            |



## Ternary Operator

The ternary operator is a conditional operator that works just like an `if-then-else` statement but in shorthand syntax.  It has three operands:

```
condition ? value1 if true : value2 if false
```

The `condition` must evaluate to a `Boolean` value.  If `true` then the `value1` will be used, or else `value2` will be used.  You can combine this operator with parenthesis, Elvis operators, etc., to build rich expressions.

```javascript
result = ( 10 > 0 ) ? true : false
result = animal eq 'dog' ? 'bark' : 'not a dog'

// More complex approach
result = creditScore > 800 ? "Excellent" :
    ( creditScore > 700 ) ? "Good" :
    ( creditScore > 600 ) ? "Average" : "Bad"
```



## Elvis Operator (Null Coalescing)

The Elvis operator is usually referred to as the [null coalescing operator](https://en.wikipedia.org/wiki/Null\_coalescing\_operator).  Its name comes from the symbol it represents, which looks like Elivs hair turned sideways: `?:`.  If the expression to the operator's left is `null` , then the expression on the right will be evaluated as the result of the expression.

```
expression ?: defaultValueOrExpression
```

Here is a simple example:

```javascript
function process( result ){
    writeOutput( result ?: "nothing passed" )
}
process() // produces 'nothing passed'
process( "hello" ) // produces 'hello'

displayName = rc.name ?: 'Anonymous'

event
    .getResponse()
    .setError( true )
    .setData( rc.id ?: "" )
```

{% hint style="danger" %}
Please note that we have seen inconsistencies in both Adobe and Lucee engines regarding the implementation of this operator.  I would avoid using it in Adobe 2018 as it is broken in several cases.
{% endhint %}

## Function Operators

In CFML, functions can act as operators as well, as you can use the results of the function call as the operands.  **Function arguments can also act as expressions, and you can even pass more functions into functions as arguments or even return functions from functions.  Now that's a fun tongue twister.**

```javascript
results = ucase( "this is text " ) & toString( 12 + 50 )

// I can also pass lambdas or anonymous functions as arguments
results = listener( 2 * 3, (result) => result + 1 )
```

## Collections Operators

Many operators can work on collection objects like arrays, structs, and queries.  So let's start investigating them.

### Safe Navigation Operator

The [Safe Navigation operator](https://en.wikipedia.org/wiki/Safe\_navigation\_operator) avoids accessing a key in a structure or a value in an object that does `null` or doesn't exist. Typically when you have a reference to an object, you might need to verify that it exists before accessing the methods or properties of the object. To avoid this, the safe navigation operator will return `null` instead of throwing an exception, like so:

```javascript
var user = userService.findById( id )
// If user is not found, then this will still work but no exception is thrown.
echo( user?.getSalary() )

s = { name : "luis" }
echo( s.name )
echo( s?.name )
```

### Spread Operator

The spread operator allows an iterable object to expand and merge in certain declarations in code.  These objects in CFML are mostly arrays and structures.  This operator can quickly merge all or parts of an existing array or object into another array or object.  This operator is used by leveraging three dots `...` in specific expressions.

```javascript
// Spread
var variableName = [ ...myArray ]
// Traditional
var variableName = [].append( myArray )

// Spread
var mergedObject = { ...obj1, ...obj2 }
// Traditional
mergedObject.append( obj1 ).append( obj2 )
```

You can accomplish the result of the spread operator with the `append()` member function or traditional function in a very elegant and user-friendly syntax.  It also allows you NOT to do chaining but inline expressions.

The Spread syntax also allows an iterable such as an array expression or string, to be expanded in places where zero or more arguments (for function calls) are expected.  Here are some examples to help you understand this operator:

#### Function Calls

```javascript
numbers = [ 1, 2, 3 ]
function sum( x, y, z ){
    return x + y + z;
}
// Call the function using the spread operator
results = sum( ...numbers ) // 6

// Ignore the others
numbers = [ 1, 2, 3, 4, 5 ]
results = sum( ...numbers ) // 6
```

#### Array Definitions

```javascript
numbers = [ 1, 2, 3 ]
myArray = [ 3, 4, ...numbers ]
myArray2 = [ ...numbers ]
myArray2 = [ ...numbers, 4, 66 ]
```

#### Struct Definitions

```javascript
var mergedObject = { ...obj1, ...obj2 }

user1 = { name : "luis", age: 15 }
user2 = { name : "joe", location : "miami" }

mergedUsers = { ...user1, ...user2 }
// What will the output be?
writeDump( mergedUsers )
// { name : "joe" , age : 15, location : "miami" }
```

### Rest Operator

{% hint style="danger" %}
Only available in ACF 2021+ and for function arguments
{% endhint %}

The Rest function operator is similar to Spread Operator but behaves oppositely. The spread syntax expands the iterable constructs into individual elements, and the Rest syntax collects and condenses them into a single construct, usually an array.  Please note that this operator only works on function arguments as of now.

Imagine I need to create a function that takes in an unlimited number of Identifiers, so I can return all items that have that ID:

```javascript
function findById( ...ids ){
}

findById( 1 ) // ids is a single value of 1
findById( 1, 23, 34, 456 ) // ids is an array of values
```

You can also combine them in functions with other arguments:

```java
function findById( entityName, ...ids ){
}

findById( "User", 1 ) // ids is a single value of 1
findById( "Car", 1, 23, 34, 456 ) // ids is an array of values
```

