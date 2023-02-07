---
description: Integers and floats to rule the world!
---

# Numbers

There are two basic kinds of numbers in CFML: **integers** (whole numbers) and **floats** (have a decimal point). Internally, each CFML engine treats them uniquely and backs up each numerical value as a Java class: `java.lang.Double` or `java.lang.Integer`.

| Type      | Size (bits) | Min Value             | Max Value               |
| --------- | ----------- | --------------------- | ----------------------- |
| `Integer` | 32          | -2,147,483,648 (-231) | 2,147,483,647 (231 - 1) |

| Type     | Size (bits) | Significant Bits | Exponent Bits | Decimal Digits |
| -------- | ----------- | ---------------- | ------------- | -------------- |
| `Double` | 64          | 53               | 11            | 15-16          |

{% hint style="danger" %}
Lucee stores all numerical values as Doubles
{% endhint %}

{% hint style="danger" %}
Adobe stores integers as Integer and floats as Doubles
{% endhint %}

{% hint style="success" %}
**Tip:** If you are dealing with currency or tracking precision, please read about `precisionEvaluate()` to represent big numbers and precision results: [https://cfdocs.org/precisionevaluate](https://cfdocs.org/precisionevaluate)
{% endhint %}

```javascript
a = 1;
b = 50.1;
writeOutput( a * b );
```

Also note that CFML will do the auto-casting for you when converting between integers and doubles.

## Numeric Type

Once we start looking at functions/closures and lambdas, you will see that you can also type the incoming arguments and results of functions.  You also won't need to type it with integer or float, just as `numeric:`

```javascript
numeric function add( numeric a, numeric b ){
    return a + b;
}
```

## Operators & Functions

CFML offers tons of mathematical [operators](operators.md#arithmetic-operators) and functions: [https://cfdocs.org/math%2Dfunctions](https://cfdocs.org/math-functions)

| abs               | aCos           | arrayAvg       |
| ----------------- | -------------- | -------------- |
| arraySum          | aSin           | atn            |
| bitAnd            | bitMaskClear   | bitMaskRead    |
| bitMaskSet        | bitNot         | bitOr          |
| bitSHLN           | bitSHRN        | bitXor         |
| ceiling           | cos            | decrementValue |
| expt              | fix            | floor          |
| formatBaseN       | incrementValue | inputBaseN     |
| int               | log            | log10          |
| max               | min            | pi             |
| precisionEvaluate | rand           | randomize      |
| randRange         | round          | sgn            |
| sin               | sqr            | tan            |

## Casting

CFML also has the `toNumeric()` function that you can use to cast a value to a number using different [radixes](https://en.wikipedia.org/wiki/Radix).&#x20;

{% hint style="info" %}
In a [positional numeral system](https://en.wikipedia.org/wiki/Positional\_numeral\_system), the radix or base is the number of unique [digits](https://en.wikipedia.org/wiki/Numerical\_digit), including the digit zero, used to represent numbers. For example, for the [decimal system](https://en.wikipedia.org/wiki/Decimal) (the most common system in use today) the radix is ten, because it uses the ten digits from 0 through 9.
{% endhint %}

## Repeating Instructions

Number variables can be used to repeat instructions. Like in many other languages, CFML supports the `for`, `while` and `loop` constructs:

```javascript
for( var i = 0; i <= 10; i++ ){
    writeOutput( "Showing day " & i );
}

i =1;
while( i <= 10 ){
    writeOutput( "Showing day " & i++ );
}
```

{% hint style="info" %}
Please note that the syntax varies from tag to script, so refer to the docs for subtle differences. Please also note that you can iterate over structures, arrays, queries, and objects in CFML; we will see this in later sections.

See [https://cfdocs.org/cfloop](https://cfdocs.org/cfloop), [https://cfdocs.org/cfwhile](https://cfdocs.org/cfwhile) for more information
{% endhint %}
