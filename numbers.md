# Numbers

There are two basic kinds of numbers: **integers** (whole numbers) and **floats** (have a decimal point).  Internally, each CFML engine treats them in their own way.  However, all we must know is how to operate them with typical math operators: `+ `, `-`, `*`, `/`.  However, there are many Java mathematical member functions and [mathematical functions](https://cfdocs.org/math-functions) available to you in CFML.

> **Tip**: If you are dealing with currency or tracking precision, please read about `precisionEvaluate()` to represent big numbers and precision results: https://cfdocs.org/precisionevaluate


## Increment/Decrement

CFML gives you two additional operators for working with numbers:

* `++` - Increment a number
* `--` - Decrement a number

It is also important to note that you can add these operators to the variables or numbers directly.  You can also add them before the instruction and after the instruction with different effects:

### After Operation

If you add a `++,--` after the declaration, then CFML will use your number as is and once that operation is done, it will increment or decrement by 1 the value of the variable.

```js
a = 1;
writeOutput( a++ );
writeOutput( a++ );
```