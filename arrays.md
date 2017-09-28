# Arrays

Almost every programming language allows you to represent different types of collections.  In CFML we have to types of collections: arrays and structures.

An array is a number-indexed list. Imagine you had a blank piece of paper and drew a set of three small boxes in a line:


```
 ---  ---  ---
|   ||   ||   |
 ---  ---  ---
```

You could number each one by its position left to right:

```
 ---  ---  ---
|   ||   ||   |
 ---  ---  ---
  1    2    3
```

Then put strings in each box:

```
 -------------  ---------  ----------
| "Breakfast" || "Lunch" || "Dinner" |
 -------------  ---------  ----------
       1            2           3
```

We have a three element Array. CFML arrays can grow and shrink dynamically at runtime just like Array Lists or Vectors in Java, so if we added an element it’d usually go on the end or appended at the end.

```
 -------------  ---------  ----------  -----------
| "Breakfast" || "Lunch" || "Dinner" || "Dessert" |
 -------------  ---------  ----------  -----------
       1            2           3           4
```

If you asked the array for the element in position two you’d get back `Lunch`. Ask for the last element and you’d get back `Dessert`.

## The Story of One

Now, have you detected something funny with the ordering of the elements? Come on, look closer....... They start with `1` and not `0`, now isn't that funny.  CFML is one of the few languages were array indexes start at `1` and not `0`.  So if you have a PHP, Ruby or Java background, remember that `1` is were you start.

## Arrays in Code

Let's go ahead and model some code in CFML using our fancy REPL tool CommandBox:



