# HTTP/S Calls

CFML makes it really **easy** to interact with **any** HTTP/S endpoint via the `cfhttp` tag/construct \([https://cfdocs.org/cfhttp](https://cfdocs.org/cfhttp)\). The `cfhttp` call will generate an HTTP/S request and parse the response into a nice CFML structure. 

```java
cfhttp( url="https://www.google.com/", result="result" ){
    cfhttpparam( name="q", type="formfield", value="cfml" )
}
writeDump( result )
```

{% hint style="info" %}
You can use ANY http method in the `cfhttp` calls, the default is a `GET` operation.
{% endhint %}

As you can see from the example above, you can pass parameters to the HTTP request by using the child `cfhttpaparam` construct.  This parameter can be of many different types: `header, body, xml, cgi, file, url, formfield, cookie` depending on the requirements of the http endpoint.

## The Result Structure

The result structure will contain the following keys:

| Key | Description |
| :---: | :--- |
| `statusCode` | The HTTP response code and reason string. |
| `fileContent` | The body of the HTTP response. Usually a string, but could also be a Byte Array. |
| `responseHeader` | A structure of response headers, the keys are header names and the values are either the header value or an array of values if multiple headers with the same name exist. |
| `errorDetail` | An error message if applicable. |
| `mimeType` | The mime type returned in the Content-Type response header. |
| `text` | A boolean indicateing if the response body is text or binary |
| `charset` | The character set returned in the Content-Type header. |
| `header` | All the http response headers as a single string. |

## CFHTTP Arguments

This construct accepts many arguments with different features you can use when executing http/s calls, below we list just the most common ones, you can find them all here: [https://cfdocs.org/cfhttp](https://cfdocs.org/cfhttp)

| Argument | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `url` | URL |  | The http/s endpoint to hit |
| `port` | numeric | 80/443 | The port of the endpoint to hit. 80 for http and 443 for https |
| `method` | string | GET | The http method to use. |
| `username` | string |  | An optional server username |
| `password` | string |  | An optional server password |
| `useragent` | string | ColdFusion | The user agent to simulate for the request |
| `charset` | string | utf-8 | The encoding to use |
| `resolveUrl` | boolean | false | No does not resolve URLs in the response body. As a result, any relative URL links in the response body do not work. Yes resolves URLs in the response body to absolute URLs, including the port number, so that links in a retrieved page remain functional. |
| `redirect` | boolean | true | If the response header includes a [Location](https://cfdocs.org/location) field, determines whether to redirect execution to the URL specified in the field. |
| `timeout` | numeric | unlimited | A value in seconds of the max time to take for the request. |
| `getAsBinary` | string | auto | If **yes**, convert to CFML binary type, **No** keep as text, **auto** let CFML detect and convert as necessary |
| `result` | string | cfhttp | The name of the variable you want the result structured returned into |
| `multipart` | boolean | false | Tells ColdFusion to send all data specified by [cfhttpparam](https://cfdocs.org/cfhttpparam) type="formField" tags as multipart form data, with a Content-Type of multipart/form-data. |

Basically, you can do any type of http/s calls and consume any type of RESTFul webservices with a nice CFML syntax!

## CFHTTPParam

As mentioned before in our example we can use the `cfhttpparam` construct to pass parameters to the http/s endpoint.  The parameters can be of different types as we can see in the following table.

```java
cfhttpParam( type="", name="", value="", file="", encoded="", mimetype="" );
```

### Param Types

| Type | Description |
| :--- | :--- |
| `header` | Specifies an HTTP header. Does not URL encode the value |
| `body` | Specifies that the `value` is the body of the HTTP request. |
| `xml` | Identifies the request as having a content-type of  `text/xml` and specifies that the `value` attribute contains the body of the HTTP request. |
| `cgi` | Same as `header` but URL encodes the `value` by default. |
| `file` | Tells CFML to send the contents of the specified file. |
| `url` | Specifies a URL query string name-value pair to append to the [cfhttp](https://cfdocs.org/cfhttp) url attribute. URL encodes the value. |
| `formfield` | Specifies a form field to send. URL encodes the value by default. |
| `cookie` | Specifies a cookie to send as an HTTP header. URL encodes the value. |

### Param Arguments

The available param arguments to the cfhttpparam construct are:

| Argument | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `type` | string |  | The type of data from the available types above |
| `name` | string |  | The variable name for the data |
| `value` | string |  | The value of the variable |
| `file` | path |  |  Applies to `file` type; ignored for all other types. The absolute path to the file that is sent with the request. |
| `encoded` | boolean | false | Applies to `formfield` and `cgi` types; ignored for all other  types. Specifies whether to [URLEncode](https://cfdocs.org/urlencode) the form field or  header. |
| `mimetype` | string |  | Applies to `file` type; invalid for all other types.  Specifies the MIME media type of the file contents.  The content type can include an identifier for the  character encoding of the file; for example, text/html;  charset=ISO-8859-1 indicates that the file is HTML text in  the ISO Latin-1 character encoding. |

Here is another example for you:

```java
cfhttp( url="https://myrestapp.com/user", result="local.result", method="post" ){
        cfhttpparam( name="x-api-token", type="header", value="123" )
        cfhttpparam( 
                type="body", 
                value=serializeJson( '{
                        name : "luis",
                         age : 2
                }' )
        )
}
writeDump( result )
```

## Hyper : HTTP Builder

Leveraging `cfhttp` is very very easy to use. However, it can be cumbersome and not necessarily fluent or object oriented.  For this, we have provided a module called [Hyper](https://forgebox.io/view/hyper) which can help you build fluent and amazing HTTP Builders \([https://forgebox.io/view/hyper](https://forgebox.io/view/hyper)\)

Hyper was built after coding several API SDK's for various platforms — [S3SDK](https://github.com/coldbox-modules/s3sdk), [cbstripe](https://github.com/coldbox-modules/cbox-stripe), and [cbgithub](https://github.com/elpete/cbgithub), to name a few. I noticed that I spent a lot of time setting up the plumbing for the requests and a wrapper around `cfhttp`. Each implementation was mostly the same but slightly different. It was additionally frustrating because I really only needed to tweak a few values, usually just the `Authorization` header. It would be nice to create an HTTP client pre-configured for each of these SDK's. It seemed the perfect fit for a module.

### The problem it solves

Hyper exists to provide a fluent builder experience for HTTP requests and responses. It also provides a powerful way to create clients, Bulider objects with pre-configured defaults like a base URL or certain headers.

### HyperBuilder

The component you will most likely inject is the `HyperBuilder`. This is commonly aliased as `hyper`.

```java
component {
    property name="hyper" inject="HyperBuilder@Hyper";
}
```

The `HyperBuilder` creates new requests. This can be done in one of two ways:

1. Calling the `new` method will create a new request with the configured defaults.
2. Calling any method on `HyperRequest` on the `HyperBuilder` instance will create a new request and forward on the method call.

Using the `HyperBuilder` lets you easily create requests with defaults while also avoiding having to deal with providers directly.

### HyperRequest

Though the `HyperBuilder` is the component you will most likely inject, `HyperRequest` is the component will you interact with the most. `HyperRequest` provides a fluent interface to configure your HTTP call.

**Example:**

```java
hyper.get( "https//api.github.com/users" );

hyper.setMethod( "PUT" )
    .withHeaders( { "Authorization" = "Bearer #token#" } )
    .setUrl( "https://jsonplaceholder.typicode.com/posts/1" )
    .setBody( {
        title: "New Title"
    } )
    .send();
```

### Request Defaults

Hyper allows you to configure defaults for your requests. This is particularly useful for reducing boilerplate in your application.

Defaults are set on the `HyperBuilder` instance. The easiest way to do this is to configure it in WireBox:

```java
// config/WireBox.cfc
component {

    function configure() {
        map( "StarWarsClient" )
            .to( "hyper.models.HyperBuilder" )
            .asSingleton()
            .initWith(
                baseUrl = "https://swapi.co/api"
            );
    }

}
```

Now, you can inject this pre-configured builder wherever you need in your application:

```java
component {

    property name="StarWarsClient" inject="id";

    function findUser( id ) {
        return StarWarsClient.get( "/people/#id#" );
    }

}
```

You can even create multiple clients using this approach:

```java
// config/WireBox.cfc
component {

    function configure() {
        map( "SWAPIClient" )
            .to( "hyper.models.HyperBuilder" )
            .asSingleton()
            .initWith(
                baseUrl = "https://swapi.co/api"
            );

        map( "GitHubClient" )
            .to( "hyper.models.HyperBuilder" )
            .asSingleton()
            .initWith(
                baseUrl = "https://api.github.com",
                headers = {
                    "Authorization" = getSetting( "SWAPI_TOKEN" )
                }
            );
    }

}
```

You can also set or change the defaults by either passing the key / value pairs in to the `init` method or by calling the appropriate `HyperRequest` method on the `HyperBuilder.defaults` property.

```java
var hyper = new Hyper.models.HyperBuilder(
    baseUrl = "https://api.github.com"
);
hyper.defaults.withHeaders( { "Authorization" = token } );
```





