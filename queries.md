# Queries

CFML became famous in its infancy with how easy it was to query databases with a simple `cfquery` tag. No ceremony, just a plain datasource definition in the administrator and we could query the database with ease.

In modern times, we have many more ways to query the database and defining datasources can occur not only in the admin but in our application's `Application.cfc`. See [Application.cfc](/applicationcfc.md) for more information.

## What is a query?

A query is a request to a database. It returns a CFML `query` object containing a **recordset** and other metadata information about the query. The query can ask for information from the database, write new data to the database, update existing information in the database, or delete records from the database. This can be done in several ways:

* Using the `cfquery` tag or function construct. (https://cfdocs.org/cfquery)
* Using the `queryExecute()` function. (https://cfdocs.org/queryexecute)

```
// Tag syntax
<cfquery name = "qItems" datasource="pantry"> 
 SELECT QUANTITY, ITEM 
 FROM CUPBOARD 
 ORDER BY ITEM 
</cfquery> 

// New script syntax
var q = new Query( datasource="pantry" );
q.setSQL( "
SELECT QUANTITY, ITEM 
FROM CUPBOARD
ORDER BY ITEM
" );
// Execute it
qItems = q.execute().getResult();  

// Yet another script alternative

qItems = queryExecute( 
 sql = "SELECT QUANTITY, ITEM FROM CUPBOARD ORDER BY ITEM"
);

```

> **Info** on Lucee, the `datasource` can even be defined inline.

Please note that when using the `Query` object, you can pass many attributes into the constructor, chain and much more.  Please see the docs for further syntax options: https://helpx.adobe.com/coldfusion/cfml-reference/script-functions-implemented-as-cfcs/query.html.  You can also omit the `datasouce` completely from query calls and CFML will use the one defined in `Application.cfc` as the default datasource connection.

```java
this.datasource = "pantry";
```

## Displaying Results

The query object can be iterated on like a normal collection through a `for, cfloop or cfoutput` construct.

**In a Template**

```
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

Most of the time we won't have the luxury of simple queries, we will need user input in order to construct our queries.  Here is where you need to be extra careful as to not allow for SQL injection.  CFML has several ways to help you prevent SQL Injection whether using tags or script calls.  Levarage the `cfqueryparam` construct/tag (https://cfdocs.org/cfqueryparam) and always sanitize your input via the `encode` functions in CFML.

```java
q = new Query(
 sql = "select quantity, item from cupboard where item_id = :itemID"
);

q.addParam( 
 name      = "itemID",
 cfsqltype = "CF_SQL_VARCHAR",
 value     = arguments.itemID,
 list      = true
);

qItems = q.execute().getResult(); 

queryExecute(
 "select quantity, item from cupboard where item_id = :itemID"
 { itemID = arguments.itemID }
);

queryExecute(
 "select quantity, item from cupboard where item_id = ?"
 [ arguments.itemID ]
);

```

You can use the `:varname` notation in your SQL construct to denote a variable place holder or a `?` to denote a positional placeholder.