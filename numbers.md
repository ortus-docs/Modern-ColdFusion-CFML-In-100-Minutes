# Numbers

There are two basic kinds of numbers: **integers** (whole numbers) and **floats** (have a decimal point).  Internally, each CFML engine treats them in their own way.  However, all we must know is how to operate them with typical math operators: `+ `, `-`, `*`, `/`.  However, there are many Java mathematical member functions and [mathematical functions](https://cfdocs.org/math-functions) available to you in CFML.

> **Tip**: If you are dealing with currency or tracking precision, please read about `precisionEvaluate()` to represent big numbers and precision results: https://cfdocs.org/precisionevaluate


```js
a = 1;
b = 50.1;
writeOutput( a * b );
```


## Increment/Decrement

CFML gives you two additional operators for working with numbers:

* `++` - Increment a number
* `--` - Decrement a number

It is also important to note that you can add these operators to the variables or numbers directly.  You can also add them before the instruction and after the instruction with different effects:

### After Operations

If you add a `++,--` after the declaration, then CFML will use your number as is and once that operation is done, it will increment or decrement by 1 the value of the variable.

```js
a = 1;
writeOutput( a++ ); => 1
writeOutput( a ); => 2
```

As you can see from the example above, in line 2 the output will be still 1, but after that operation `a` will be incremented. So the next usage will reflect it.  Same will go for the `--` operator used after operations.


### Before Operations

If you add a `++,--` before the declaration, then CFML will increment/decrement the number before the operation is used.

```js
a = 1;
writeOutput( ++a ); => 2
writeOutput( a ); => 2
```

As you can see from the example above, in line 2 the output will be now 2, as `a` was incremented before. Same will go for the `--` operator used before operations.

## Repeating Instructions

Number variables can be used to repeat instructions.  Like in many other languages, CFML supports the `for` loop and `while` loops.

```js
for( var i = 0; i lte 10; i++ ){
    writeOutput( "Showing day " & i );
}
```