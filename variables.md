# Variables

In CFML, variables are just pointers to a piece of data.  They can hold **any** value you like and change it at runtime.  In some languages, you need to specify the type of data you want your variable to hold.  In CFML, you do not need to assign one as everything is dynamic.  The Lucee server even goes further and infers types according to the initial value you assign your variable.

Please note that assignments are evaluated from right to left instead of traditional reading which is from left to right.

Open up the CommandBox Shell and go into **REPL** mode by typing `repl`.  Every time you assign a value to a variable, the CommandBox REPL will output or echo the variable for you.

![](/assets/variables.png)

As you can see, we can create strings, numerics, arrays, structs and so much more.  No need for types or special assignments.

## Flexible Typing

You can also create a variable with one type and then switch it to another dynamically:

![](/assets/flexible-typing.png)

As you can see, the last equality wins! In this case, `a` is now an array.

