# Variable Scopes

In the CFML language, there are many persistence and visibility scopes that exist for variables to be placed in.  These are differentiated by context: in a CFC, in a function, tag, thread or in a template.  All CFML scopes are implemented as structures of key-value name pairs. The default scope for variable storage is called `variables`.  Thus you can refer variables like this:

```js
a = hello
writeOutput( a )
or 
writeOutput( variables.a )
```

We will study these scopes further, but here are the major scopes delineated by context:

## Persistence Scopes

Can be used in any context, used for persisting variables for a period of time.

* `session` - stored in server RAM or external storages tracked by unique web visitor
* `client` - stored in cookies, databases, or external storages (simple values only)
* `application` - stored in server RAM or external storage tracked by the running ColdFusion application
* `cookie` - stored in a visitor's browser
* `server` - stored in server RAM for ANY application for that CFML instance
* `request` - stored in RAM for a specific user request ONLY
* `cgi` - read only scope provided by the servlet container and CFML
* `form` - Variables submitted via HTTP posts
* `URL` - Variables incoming via HTTP GET operations or the incoming URL

## Template Scopes

* `variables` - The default scope where all variables are assigned to.

## Component Scopes

* variables - Private scope, visible internally to the CFC
* this - Public scope, visible from the outside world
* static - No need for a CFC instance, available as a CFC representation

## Function Scopes
* variables - Has access to private variables within a Component or Page
* this - Has access to public variables within a Component or Page
* local - Function scoped variables, only exist within the function execution. Referred to as `var` scoping
* arguments - Incoming variables to a function

## Tag Scopes
* `attributes` - Incoming tag attributes
* `variables` - The default scope for variable assignments
* `caller` - Used within a custom tag to set or read variables within the template that called it.


## Thread Scopes
* attributes - Passed variables via a thread
* thread - A thread specific scope that can be used for storage and retrieval
* local - Variables local to the thread context

