# Queries

CFML became famous in its infancy with how easy it was to query databases with a simple `cfquery` tag. No ceremony, just a plain datasource definition in the administrator and we could query the database with ease.

In modern times, we have many more ways to query the database and defining datasources can occur not only in the admin but in our application's `Application.cfc` or even define it at runtime. See [Application.cfc](../rapid-app-development/applicationcfc.md) for more information.

## What is a query?

A query is a request to a database. It returns a CFML `query` object containing a **recordset** and other metadata information about the query. The query can ask for information from the database, write new data to the database, update existing information in the database, or delete records from the database. This can be done in several ways:

* Using the `cfquery` tag or function construct. \([https://cfdocs.org/cfquery](https://cfdocs.org/cfquery)\)
* Using the `queryExecute()` function. \([https://cfdocs.org/queryexecute](https://cfdocs.org/queryexecute)\)

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
```

> **Info** on Lucee, the `datasource` can even be defined inline.

Please note that when using the `Query` object, you can pass many attributes into the constructor, chain and much more. Please see the docs for further syntax options: [https://helpx.adobe.com/coldfusion/cfml-reference/script-functions-implemented-as-cfcs/query.html](https://helpx.adobe.com/coldfusion/cfml-reference/script-functions-implemented-as-cfcs/query.html). You can also omit the `datasouce` completely from query calls and CFML will use the one defined in `Application.cfc` as the default datasource connection.

```java
this.datasource = "pantry";
```

## Displaying Results

The query object can be iterated on like a normal collection through a `for, cfloop or cfoutput` construct.

**In a Template**

```javascript
<cfoutput query = "qItems">
There are #qItems.Quantity# #qItems.Item# in the pantry<br />
</cfoutput>
```

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

As you can see, there are many ways to iterate over the query. Choose the approach that suits your needs.

## Using Input

Most of the time we won't have the luxury of simple queries, we will need user input in order to construct our queries. Here is where you need to be extra careful as to not allow for SQL injection. CFML has several ways to help you prevent SQL Injection whether using tags or script calls. Levarage the `cfqueryparam` construct/tag \([https://cfdocs.org/cfqueryparam](https://cfdocs.org/cfqueryparam)\) and always sanitize your input via the `encode` functions in CFML.

```java
queryExecute(
 "select quantity, item from cupboard where item_id = :itemID"
 { itemID = { value=arguments.itemID, cfsqltype="cf_sql_varchar", list=true } }
);

queryExecute(
 "select quantity, item from cupboard where item_id = ?"
 [ { value=arguments.itemID, cfsqltype="cf_sql_varchar", list=true } ]
);
```

You can use the `:varname` notation in your SQL construct to denote a variable place holder or a `?` to denote a positional placeholder.

## Query Methods

There are also several query methods available in CFML that can help you manage queries but also create them on the fly \([https://cfdocs.org/query-functions](https://cfdocs.org/query-functions)\). Please note that you can also use chaining and member functions as well.

* queryNew\(\)
* queryAddRow\(\)
* queryAddColumn\(\)
* queryColumnArray\(\)
* queryColumnCount\(\)
* queryColumnData\(\)
* queryColumnExists\(\)
* queryColumnList\(\)
* queryCurrentRow\(\)
* queryDeleteColumn\(\)
* queryDeleteRow\(\)
* queryEach\(\)
* queryEvery\(\)
* queryFilter\(\)
* queryGetCell\(\)
* queryGetResult\(\)
* queryGetRow\(\)
* queryMap\(\)
* queryRecordCount\(\)
* queryReduce\(\)
* queryRowData\(\)
* querySetCell\(\)
* querySlice\(\)
* querySome\(\)
* querySort\(\)
* quotedValueList\(\)
* valueList\(\)

## Building Queries

You can use a combination of the methods above to create your own queries

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

## Returning Arrays of Structs or Struct of Structs

In the Lucee CFML engine \(coming soon to Adobe\), you can also determine the return type of database queries to be something other than the CFML query object. You can choose array of structs or struct of structs. This is fantastic for modern applications that rely on rich Javascript frameworks and producing JSON.

```java
users = queryNew( "firstname", "varchar", [{"firstname":"Han"}] );
subUsers = queryExecute( "select * from users", {}, { dbtype="query", returntype="array" } );
writedump( subUsers ); 

users = queryNew( "id, firstname", "integer, varchar", [{"id":1, "firstname":"Han"}] );
subUsers = queryExecute( "select * from users", {}, { dbtype="query", returntype="struct", columnkey="id" } );
writedump( subUsers );
```

## QB = Query Builder

We have created a fantastic module to deal with queries in a fluent and elegant manner. We call it **QB** short for query builder \([https://www.forgebox.io/view/qb](https://www.forgebox.io/view/qb)\). You can install it using CommandBox into your application by just saying:

```text
box install qb
```

Using qb, you can:

* Quickly scaffold simple queries
* Make complex, out-of-order queries possible
* Abstract away differences between database engines

```java
// qb
query = wirebox.getInstance('Builder@qb');
q = query.from( 'posts' )
         .whereNotNull( 'published_at' )
         .whereIn( 'author_id', [5, 10, 27] )
         .get();
```

You can find all the documentation in our Ortus Books docs: [http://qb.ortusbooks.com/](http://qb.ortusbooks.com/)

