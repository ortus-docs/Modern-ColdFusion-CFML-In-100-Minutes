---
description: CFML provides the easiest way to query a database
---

# Database Queries

CFML became famous in its infancy because it was easy to query databases with a simple `cfquery` tag and no verbose ceremonious coding. There is no ceremony, just a plain datasource definition in the administrator, and we could easily query the database.

In modern times, we have many more ways to query the database, and defining data sources can occur not only in the admin but in our web application's `Application.cfc` or even define it at runtime programmatically or within the query constructs themselves.

{% hint style="info" %}
See [Application.cfc](../beyond-the-100/applicationcfc.md) for more information on how to leverage it for web development.
{% endhint %}

## What is a Datasource?

A datasource is a **named** connection to a specific database with specified credentials. You can define an infinite amount of data sources in your CFML applications in the following locations:

* Global ColdFusion Engine (Adobe or Lucee) Administrator
  * **Adobe** : `http://localhost:port/CFIDE/adminstrator`
  * **Lucee**: `http://localhost:port/lucee/admin/server.cfm`
* The `Application.cfc`, which will dictate the data sources for that specific ColdFusion application
* Inline in `cfquery` or `queryexecute` calls

The datasource is then used to control the database's connection pool and allow the ColdFusion engine to execute JDBC calls against it.

## What is a query?

A query is a request to a database representing the results' rows and columns. It returns a CFML `query` object containing a **record set** and other metadata information about the query. The query can ask for information from the database, write new data to the database, update existing information in the database, or delete records from the database. This can be done in several ways:

* Using the `cfquery` tag. ([https://cfdocs.org/cfquery](https://cfdocs.org/cfquery))
* Using the `queryExecute()` function. ([https://cfdocs.org/queryexecute](https://cfdocs.org/queryexecute))

{% hint style="info" %}
In Lucee, a query is backed by the following class: `lucee.runtime.type.QueryImpl`\
``In Adobe, a query is backed by the following class: `coldfusion.sql.QueryTable`
{% endhint %}

```javascript
// Tag syntax
<cfquery name = "qItems" datasource="pantry"> 
 SELECT QUANTITY, ITEM 
 FROM CUPBOARD 
 ORDER BY ITEM 
</cfquery> 

// script syntax

qItems = queryExecute( 
 "SELECT QUANTITY, ITEM FROM CUPBOARD ORDER BY ITEM"
);

// Lucee datasource inline definition
queryExecute(
  "SELECT * FROM Employees WHERE empid = ? AND country = ?", // sql
  [ 1, "USA" ], // params
  { // options
    datasource : {
      class : "com.microsoft.sqlserver.jdbc.SQLServerDriver",
      connectionString : "jdbc:sqlserver://#getSystemSetting("DB_CONNECTIONSTRING")#",
      username : getSystemSetting("DB_USER"),
      password : getSystemSetting("DB_PASSWORD")
    }
  }
)
```

{% hint style="success" %}
If you are using **Lucee**, the datasource can even be defined inline. So instead of giving the name of the `datasource` it can be a `struct` definition of the datasource you want to connect to, just like the struct in `Application.cfc`
{% endhint %}

## Default Datasource

You can also omit the `datasource` completely from query calls, and CFML will use the one defined in `Application.cfc` as the **default** datasource connection. This is a great way to encapsulate the datasource in a single location. However, we all know that there could be some applications with multiple data sources; that's ok; at least you can have one by default.

{% code title="Application.cfc" %}
```java
component{
    this.name = "myApp";

    // Default Datasource Name
    this.datasource = "pantry";

}
```
{% endcode %}

## Defining Datasources

If you want to use the ColdFusion Engine's administrators for registering data sources, you must visit each administrator's interfaces and follow their wizards.

### ColdFusion Engine Administrator

{% embed url="https://docs.lucee.org/guides/cookbooks/datasource-define-datasource.html" %}

{% embed url="https://helpx.adobe.com/coldfusion/configuring-administering/data-source-management-for-coldfusion.html" %}

### `Application.cfc`

You can also define the datasources in the `Application.cfc`, which is sometimes our preferred approach as the connections are versioned controlled and more visible than in the admin. You will do this by defining a struct called `this.datasources`. Each **key** will be the name of the datasource to register and the **value** of each key a struct of configuration information for the datasource. However, we recommend that you setup environment variables in order to NOT store your passwords in plain-text in your source code.

{% code title="Application.cfc" %}
```java
component{
    this.datasources = {
        // Adobe Driver Approach
        mysql = {
            database : "mysql",
            host : "localhost",
            port : "3306",
            driver : "MySQL",
            username : "root",
            password : "mysql",
            options : value
        },
        // Adobe url approach
        mysql2 = {
            driver : "mysql",
            url : "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF-8&useLegacyDatetimeCode=true",
            username : "",
            password : ""
        },
        // Shorthand Lucee Approach
        myLuceeDNS = {
            class : "com.mysql.jdbc.Driver",
            connectionString : "jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF-8&useLegacyDatetimeCode=true",
            username : "",
            password : "" 
        },
        // Long Lucee Approach
        myLuceeDNS = {
            type : "mysql",
            database : "mysql",
            host : "localhost",
            port : "3306",
            username : "",
            password : ""
        }
    };
}
```
{% endcode %}

{% hint style="success" %}
For the inline approach, you will use the struct definition, as you see in the `Application.cfc` above and pass it into the `cfquery` or `queryexecute` call.
{% endhint %}

### Portable Datasources

You can also make your data sources portable from application to application or CFML engine to engine by using our [CFConfig](https://cfconfig.ortusbooks.com/) project. CFConfig allows you to manage almost every setting that shows up in the web administrator, but instead of logging into a web interface, you can manage it from the command line by hand or as part of a scripted server setup. You can seamlessly transfer config for all the following:

* CF Mappings
* Data sources
* Mail servers
* Request, session, or application timeouts
* Licensing information (for Adobe)
*   Passwords

    \-Template caching settings

    \-Basically any settings in the web based administrator

You can easily place a `.cfconfig.json` in the web root of your project, and if you start up a CommandBox server on any CFML engine, CFConfig will transfer the configuration to the engine's innards:

{% code title=".cfconfig.json" %}
```java
{
    "requestTimeoutEnabled":true,
    "whitespaceManagement":"white-space-pref",
    "requestTimeout":"0,0,5,0",
    "cacheDefaultObject":"coldbox",
    "caches":{
        "coldbox":{
            "storage":"true",
            "type":"RAM",
            "custom":{
                "timeToIdleSeconds":"1800",
                "timeToLiveSeconds":"3600"
            },
            "class":"lucee.runtime.cache.ram.RamCache",
            "readOnly":"false"
        }
    },
    "datasources" : {
         "coldbox":{
             "host":"${DB_HOST}",
             "dbdriver":"${DB_DRIVER}",
             "database":"${DB_DATABASE}",
             "dsn":"jdbc:mysql://{host}:{port}/{database}",
             "custom":"useUnicode=true&characterEncoding=UTF-8&useLegacyDatetimeCode=true&autoReconnect=true",
             "port":"${DB_PORT}",
             "class":"${DB_CLASS}",
             "username":"${DB_USER}",
             "password":"${DB_PASSWORD}",
             "connectionLimit":"100",
             "connectionTimeout":"1"
         }
    }
}
```
{% endcode %}

{% embed url="https://cfconfig.ortusbooks.com/using-the-cli/command-overview" %}

## Displaying Results

The query object can be iterated on like a normal collection through a `for, cfloop or cfoutput` , `each()`  constructs.

{% embed url="https://cfdocs.org/cfoutput" %}

{% embed url="https://cfdocs.org/cfloop" %}

**In a CFM Template**

```xml
<cfoutput query = "qItems">
There are #qItems.Quantity# #qItems.Item# in the pantry<br />
</cfoutput>
```

You leverage the `cfoutput` tag by passing the `query` to it.  Then in the block of the tag you use dot/array notation and interpolation to output the column you want.  CFML will iterate over all rows in the query for you.

```xml
<cfoutput query = "qItems" encodeFor="html">
There are #qItems[ 'quantity' ]# #qItems[ 'item' ]# in the pantry<br />
</cfoutput>
```

{% hint style="info" %}
By specifying `encodefor="html"` each variable is encoded using the `encodeForHTML` function before it is output.
{% endhint %}

**Using Loops**

```java
for( var row in qItems ){
 systemOutput( "There are #row.quantity# #row.item# in the pantry" );
}

qItems.each( function( row, index ){
 systemOutput( "There are #row.quantity# #row.item# in the pantry" );

} );

for( var i = 1; i lte qItems.recordCount; i++ ){
 systemOutput( "There are #qItems.quantity[ i ]# #qItems.item[ i ]# in the pantry" );
}
```

As you can see, many ways to iterate over the query exist. Choose the approach that suits your needs.

### Multi-Threaded Looping

Lucee and Adobe 2021+ allow you to leverage the `each()` operations in a multi-threaded fashion. The `queryEach()` or `each()` functions allow for a `parallel` and `maxThreads` arguments so the iteration can happen concurrently on as many `maxThreads` as supported by your JVM.

```java
queryEach( array, callback, parallel:boolean, maxThreads:numeric );
each( collection, callback, parallel:boolean, maxThreads:numeric );
```

This is incredibly awesome, as now your callback will be called concurrently! However, please note that once you enter concurrency land, you should shiver and tremble. Thread concurrency will be of the utmost importance, and you must ensure that var scoping is done correctly and that appropriate locking strategies are in place.

```java
myquery.each( function( row ){
   myservice.process( row );
}, true, 20 );
```

Even though this approach to multi-threaded looping is easy, it is not performant and/or flexible.  Under the hood, the engines use a single thread executor for each execution, do not allow you to deal with exceptions, and if an exception occurs in an element processor, good luck; you will never know about it.  This approach can be verbose and error-prone, but it's easy.  You also don't control where the processing thread runs and are at the mercy of the engine. &#x20;

### ColdBox Futures Parallel Programming

If you would like a functional and much more flexible approach to multi-threaded or parallel programming, consider using the ColdBox Futures approach (usable in ANY framework or non-framework code).  You can use it by installing ColdBox or WireBox into any CFML application and leveraging our `async` programming constructs, which behind the scenes, leverage the entire Java Concurrency and Completable Futures frameworks.

{% embed url="https://coldbox.ortusbooks.com/digging-deeper/promises-async-programming/parallel-computations" %}
ColdBox Futures and Async Programming
{% endembed %}

Here are some methods that will allow you to do parallel computations:

* `all( a1, a2, ... ):Future` : This method accepts an infinite amount of future objects,  closures, or an array of closures/futures to execute them in parallel.  When you call on it, it will return a future that will retrieve an array of the results of all the operations.
* `allApply( items, fn, executor ):array` : This function can accept an array of items or a struct of items of any type and apply a function to each of the items in parallel.  The `fn` argument receives the appropriate item and must return a result.  Consider this a parallel `map()` operation.
* `anyOf( a1, a2, ... ):Future` : This method accepts an infinite amount of future objects, closures, or an array of closures/futures and will execute them in parallel. However, instead of returning all of the results in an array like `all()`, this method will return the future that executes the fastest! Race Baby!
* `withTimeout( timeout, timeUnit )` : Apply a timeout to `all()` or `allApply()` operations.  The `timeUnit` can be days, hours, microseconds, milliseconds, minutes, nanoseconds, and seconds. The default is milliseconds.

## Using Input

We usually won't have the luxury of simple queries; we will need user input to construct our queries. Here is where you need to be extra careful not to allow for [SQL injection.](https://owasp.org/www-community/attacks/SQL\_Injection) CFML has several ways to help you prevent SQL Injection, whether using tags or script calls. Leverage the `cfqueryparam` construct/tag ([https://cfdocs.org/cfqueryparam](https://cfdocs.org/cfqueryparam)) and always sanitize your input via the `encode` functions in CFML.

```java
// Named variable holder
// automatic parameterization via inline struct definitions
queryExecute(
 "select quantity, item from cupboard where item_id = :itemID"
 { itemID = { value=arguments.itemID, cfsqltype="numeric" } }
);

// Positional placeholder
queryExecute(
 "select quantity, item from cupboard where item_id = ?"
 [ { value=arguments.itemID, cfsqltype="varchar" } ]
);
```

You can use the `:varname` notation in your SQL construct to denote a **variable** placeholder or  `?` to denote a **positional** placeholder. The `cfqueryparam` tag or the inline `cfsqltype` construct will bind the value to a specific database type to avoid SQL injection and to further the database explain plan via types. The available SQL binding types are:

* `bigint`
* `bit`
* `char`
* `blob`
* `clob`
* `nclob`
* `date`
* `decimal`
* `double`
* `float`
* `idstamp`
* `integer`
* `longvarchar`
* `longnvarchar`
* `money`
* `money4`
* `nchar`
* `nvarchar`
* `numeric`
* `real`
* `refcursor`
* `smallint`
* `sqlxml`
* `time`
* `timestamp`
* `tinyint`
* `varchar`

{% hint style="warning" %}
Please note that the types can be prefixed with `cf_sql_{type}` or just used as `{type}`.
{% endhint %}

## Query Methods

Several query methods are available in CFML that can help you manage queries and create them on the fly ([https://cfdocs.org/query-functions](https://cfdocs.org/query-functions)). Please note that you can also use chaining and member functions as well.

* `queryNew()`
* `queryAddRow()`
* `queryAddColumn()`
* `queryColumnArray()`
* `queryColumnCount()`
* `queryColumnData()`
* `queryColumnExists()`
* `queryColumnList()`
* `queryCurrentRow()`
* `queryDeleteColumn()`
* `queryDeleteRow()`
* `queryEach()`
* `queryEvery()`
* `queryFilter()`
* `queryGetCell()`
* `queryGetResult()`
* `queryGetRow()`
* `queryMap()`
* `queryRecordCount()`
* `queryReduce()`
* `queryRowData()`
* `querySetCell()`
* `querySlice()`
* `querySome()`
* `querySort()`
* `quotedValueList()`
* `valueList()`

## Building Queries

You can use a combination of the methods above to create your own queries:

```java
news = queryNew("id,title", "integer,varchar");
queryAddRow(news);
querySetCell(news, "id", "1");
querySetCell(news, "title", "Dewey defeats Truman");
queryAddRow(news);
querySetCell(news, "id", "2");
querySetCell(news, "title", "Men walk on Moon");
writeDump(news);


users = queryNew( "firstname", "varchar", [{"firstname":"Han"}] );
subUsers = queryExecute( "select * from users", {}, { dbtype="query" } );
writedump( subUsers ); 

news = queryNew("id,title",
    "integer,varchar",
    [ {"id":1,"title":"Dewey defeats Truman"}, {"id":2,"title":"Man walks on Moon"} ]);
writeDump(news);

news = queryNew("id,title",
    "integer,varchar",
    {"id":1,"title":"Dewey defeats Truman"});
writeDump(news);
```

## Query of Queries

Query a local database variable without going through your database is another great way to query an already queried query. Too many queries?

```java
users = queryNew( "firstname", "varchar", [{"firstname":"Han"}] );
subUsers = queryExecute( "select * from users", {}, { dbtype="query" } );
writedump( subUsers );
```

Please note that using a query of queries can be quite slow sometimes, not all the time. An alternative approach is to use modern `queryFilter()` operations to actually filter out the necessary data from a query or `querySort()`, etc.

## Returning Arrays of Structs or Struct of Structs

In Lucee and Adobe 2021+, you can also determine the return type of database queries as something other than the CFML query object. You can choose an array of structs or a struct of structs. This is fantastic for modern applications that rely on rich JavaScript frameworks and produce JSON.

This is achieved by passing the `returntype` attribute within the query options or just an attribute of the `cfquery` tag ([https://cfdocs.org/cfquery](https://cfdocs.org/cfquery))

```java
users = queryNew( "firstname", "varchar", [{"firstname":"Han"}] );
subUsers = queryExecute( "select * from users", {}, { dbtype="query", returntype="array" } );
writedump( subUsers ); 

users = queryNew( "id, firstname", "integer, varchar", [{"id":1, "firstname":"Han"}] );
subUsers = queryExecute( "select * from users", {}, { dbtype="query", returntype="struct", columnkey="id" } );
writedump( subUsers );
```

## QB = Query Builder

We have created a fantastic module to deal with queries in a fluent and elegant manner. We call it **QB** short for **q**uery **b**uilder ([https://www.forgebox.io/view/qb](https://www.forgebox.io/view/qb)). You can install it using CommandBox into your application by just saying:

```bash
box install qb
```

Using qb, you can:

* Quickly scaffold simple queries
* Make complex, out-of-order queries possible
* Abstract away differences between database engines

```java
// qb
query = wirebox.getInstance( 'Builder@qb' );
q = query.from( 'posts' )
         .whereNotNull( 'published_at' )
         .whereIn( 'author_id', [5, 10, 27] )
         .get();
```

You can find all the documentation in our Ortus Books docs: [http://qb.ortusbooks.com/](http://qb.ortusbooks.com/)
