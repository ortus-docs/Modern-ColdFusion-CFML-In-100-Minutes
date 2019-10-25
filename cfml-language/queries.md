# Database Queries

CFML became famous in its infancy with how easy it was to query databases with a simple `cfquery` tag, no verbose ceremonious coding. No ceremony, just a plain datasource definition in the administrator and we could query the database with ease.

In modern times, we have many more ways to query the database and defining datasources can occur not only in the admin but in our application's `Application.cfc` or even define it at runtime or even within the query constructs themselves.

{% hint style="info" %}
See [Application.cfc](../beyond-the-100/applicationcfc.md) for more information on how to leverage it for web development.
{% endhint %}

## What is a Datasource?

A datasource is a **named** connection to a specific database with specified credentials.  You can define an infinite amount of datasources in your CFML applications in the following locations:

* Global ColdFusion Engine Administrator
* The `Application.cfc`, which will dictate the datasources for that specific ColdFusion application
* Inline in `cfquery` or `queryexecute` calls

The datasource is then used to control the connection pool to such database and allow for the ColdFusion engine to execute JDBC calls against it.

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

{% hint style="info" %}
If you are using **Lucee**, the datasource can even be defined inline. So instead of giving the name of the datasource it can be a struct definition of the datasource you want to connect to.
{% endhint %}

## Default Datasource

You can also omit the `datasource` completely from query calls and CFML will use the one defined in `Application.cfc`  as the **default** datasource connection. This is a great way to encapsulate the datasource in a single location.  However, we all know that there could be some applications with multiple datasources, that's ok, at least you can have one by default.

{% code-tabs %}
{% code-tabs-item title="Application.cfc" %}
```java
component{
    this.name = "myApp";
    
    // Default Datasource Name
    this.datasource = "pantry";
    
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

---

## Defining Datasources

If you want to use the ColdFusion Engine's administrators for registering datasources, you will have to visit each of the administrator's interfaces and follow their wizards.

### ColdFusion Engine Administrator

{% embed url="https://docs.lucee.org/guides/cookbooks/datasource-define-datasource.html" %}

{% embed url="https://helpx.adobe.com/coldfusion/configuring-administering/data-source-management-for-coldfusion.html" %}

### Application.cfc

You can also define the datasources in the `Application.cfc`, which is sometimes our preferred approach as the connections are versioned controlled and more visible than in the admin.  You will do this by defining a struct called `this.datasources`.  Each **key** will be the name of the datasource to register and the **value** of each key a struct of configuration information for the datasource. However, we recommend that you setup environment variables in order to NOT store your passwords in plain-text in your source code.

{% code-tabs %}
{% code-tabs-item title="Application.cfc" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="success" %}
For the inline approach, you will just use the struct definition as you see in the `Application.cfc` above and pass it into the `cfquery` or `queryexecute` call.
{% endhint %}

### Portable Datasources

You can also make your datasources portable from application to application or CFML engine to engine by using our [CFConfig](https://cfconfig.ortusbooks.com/) project.  CFConfig gives you the ability to manage most every setting that shows up in the web administrator, but instead of logging into a web interface, you can mange it from the command line by hand or as part of a scripted server setup. You can seamless transfer config for all the following:

* CF Mappings
* Datasources
* Mail servers
* Request, session, or application timeouts
* Licensing information \(for Adobe\)
* Passwords

  -Template caching settings

  -Basically any settings in the web based administrator

You can easily place a `.cfconfig.json` in the web root of your project and if you start up a CommandBox server on any CFML engine, CFConfig will transfer the configuration to the engine's innards:

{% code-tabs %}
{% code-tabs-item title=".cfconfig.json" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

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

### Multi-Threaded Looping

As of now only Lucee allows you to leverage the `each()` operations in a multi-threaded fashion.  The `queryEach()` or `each()` functions allows for a `parallel` and `maxThreads` arguments so the iteration can happen concurrently on as many `maxThreads` as supported by your JVM.

```java
queryEach( array, callback, parallel:boolean, maxThreads:numeric );
each( collection, callback, parallel:boolean, maxThreads:numeric );
```

This is incredibly awesome as now you callback will be called concurrently!  However, please note that once you enter concurrency land, you should shiver and tremble.  Thread concurrency will be of the utmost importance and you must make sure that var scoping is done correctly and that appropriate locking strategies are in place.

```java
myquery.each( function( row ){
   myservice.process( row );
}, true, 20 );
```

## Using Input

Most of the time we won't have the luxury of simple queries, we will need user input in order to construct our queries. Here is where you need to be extra careful as to not allow for SQL injection. CFML has several ways to help you prevent SQL Injection whether using tags or script calls. Leverage the `cfqueryparam` construct/tag \([https://cfdocs.org/cfqueryparam](https://cfdocs.org/cfqueryparam)\) and always sanitize your input via the `encode` functions in CFML.

```java
// Named variable holder
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

You can use the `:varname` notation in your SQL construct to denote a **variable** place holder or a `?` to denote a **positional** placeholder.  The `cfqueryparam` tag or the inline `cfsqltype` construct will bind the value to a specific database type in order to avoid SQL injection and to further the database explain plan via types.  The available SQL binding types are:

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

Please note that using query of queries can be quite slow.  An alternative approach is to use the modern `queryFilter()` operations to actually filter out the necessary data from a query, or `querySort()`, etc.

## Returning Arrays of Structs or Struct of Structs

In the Lucee CFML engine \(coming soon to Adobe\), you can also determine the return type of database queries to be something other than the CFML query object. You can choose array of structs or struct of structs. This is fantastic for modern applications that rely on rich JavaScript frameworks and producing JSON.

This is achieved by passing the `returntype` attribute within the query options or just an attribute of the cfquery tag \([https://cfdocs.org/cfquery](https://cfdocs.org/cfquery)\)

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

