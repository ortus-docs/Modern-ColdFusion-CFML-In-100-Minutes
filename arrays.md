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

![](/assets/arrays_in_code.png)

Check it out:

* The array was created by putting pieces of data between square brackets (`[]`) and separated by commas
* We added an element to the array using the member function `append()`
* We fetched the element at a specific position by using square brackets (`[ x ]`) and replaced `x` with the index we wanted
* We retrieved the size of the array by using the member function `len()`
* We searched the contents of the array using the member function `findNoCase()` and it gave us the index position of the element in the array.

Please note that all member functions can also be used as traditional [array functions](https://cfdocs.org/array-functions). However, [member functions](https://cfdocs.org/member) do look so much better for readability.


