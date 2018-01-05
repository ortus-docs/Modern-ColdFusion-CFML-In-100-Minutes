# Conditionals

Conditional statements evaluate to **true** or **false** only. The most common conditional operators are `==` (equal), `!=` (not equal), `>` (greater than), `>=` (greater than or equal to), `<` (less than), and `<=` (less than or equal to). You can also define the operators as abbreviations: `EQ, NEQ, GT, GTE, LT, and LTE`. 

Some instructions return a `true` or `false`, so they're used in conditional statements, for example, `IsArray` which is `true` only when the variable is an "array". Structures have an instruction named `structKeyExists()` or `exists()` which returns `true` if a key is present in a structure.

Also integers can be evaluated as true or false. In ColdFusion, 0 (zero) is false and any other integers are true.

<cfif 1>I am true so will show</cfif>

<cfif -2>I am true so will show</cfif>

<cfif 0>I am false so will not show</cfif>