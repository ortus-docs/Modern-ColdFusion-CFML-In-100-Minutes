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