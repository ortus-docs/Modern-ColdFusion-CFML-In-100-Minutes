# Threading

CFML allows you create asynchronous threads so you can execute a body of code in a separate thread. This is achieved via the `cfthread` tag & the `thread` construct. Threads are independent streams of execution, and multiple threads on a page can execute simultaneously and asynchronously, letting you perform asynchronous processing in CFML. CFML code within the `cfthread` tag body executes on a separate thread while the page request thread continues processing without waiting for the `cfthread` body to finish. You can allow the thread body to continue executing in the background or you can wait for it to finish.

![](../.gitbook/assets/screen-shot-2019-08-09-at-2.14.00-pm.png)

{% hint style="danger" %}
**IMPORTANT:** You cannot spawn a thread from within a thread in any of the CFML engines.
{% endhint %}

This approach is very very simplistic, if you want more control of your asynchronous programming aspects then we can move into leveraging CFML Future's via the `runAsync()` function or parallel Java streams using the [cbStreams](https://www.forgebox.io/view/cbStreams) project. Please see our [Asynchronous Programming ](../beyond-the-100/asynchronous-programming.md)section for information on advanced asynchronous programming.

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/developing-cfml-applications/using-coldfusion-threads/creating-and-managing-coldfusion-threads.html" %}

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/developing-cfml-applications/using-coldfusion-threads/working-with-threads.html" %}

### A Fair Warning

Please note that once you get into concurrency you will start to get many headaches. Your code must be thread safe, appropriate locking must be in place and overall concurrency based programming will be needed on any shared resource. It is also very difficult to debug because you are no longer in the same thread and a `writedump() + abort` combo usually goes into ether. Logging will be your best friend and outputting logs to the console for debugging purposes.

### Thread Logging

Here are some utility functions to assist with logging:

* `systemOutput( obj, addNewLine:boolean, doErrorStream:boolean)` - Writes the given text or complex objects to the output or error stream.  Complex objects are outputted as JSON. (Lucee-only) [https://cfdocs.org/systemoutput](https://cfdocs.org/systemoutput)
* `cfdump( var="text", output="console" )` - Send the variables to the output console, even complex variables. Complex objects are outputted as JSON. [https://cfdocs.org/cfdump](https://cfdocs.org/cfdump)
* `cflog( text, log, file, type ) or writeLog()` - Leverage the ColdFusion engine's logging facilities to send typed messages. [https://cfdocs.org/cflog](https://cfdocs.org/cflog)&#x20;

```java
// Lucee Only
sytemOutput( "Hello from thread land", true );
sytemOutput( myComplexObject, true );

// Dumps
writedump( var="Hello from thread land", output="console" );
writedump( var=myComplexObject, output="console" );

// Cflogs
writeLog(
    text = "Logging some info.", 
    type = "information", 
    application = "false", 
    file = "myLogFile"
);

// By default, with no file or log type it sends it to the application.log
writeLog(
    text = "Logging more info.", 
    type = "error"
);
```

## Thread Construct

Writing `thread` is extremely easy, just use the construct, give it a few attributes an boom you are in multi-threaded land. In tags you can use the `<cfthread>` tag.

```java
thread
    name = "The name of the thread, must be unique"
    action="run, sleep, join, or terminate"
    duration="The number of milliseconds to suspend the thread processing"
    priority="high, low, or normal"
    timeout="Number in milliseconds the current thread waits for this thread to finish."
{
    // Body to execute in a separate thread
    // All variables declared implicity will be placed in the thread's local scope
    // You still have access to all scopes.
}
```

Examples:

```java
thread action="run" name="myThread" {
  // do single thread stuff 
} 

// Wait for the myThread and myOtherThread to finish
thread action="join" name="myThread,myOtherThread";

// A fancy one
thread     name="#thisThreadName#"
        action="run"
        priority="#arguments.asyncPriority#"
        interceptData="#arguments.interceptData#"
        threadName="#thisThreadName#"
        buffer="#arguments.buffer#"
        key="#key#"
{

    // Retrieve interceptor to fire and local context
    var thisInterceptor = this.getInterceptors().get( attributes.key );
    var event             = variables.controller.getRequestService().getContext();

    // Check if we can execute this Interceptor
    if( variables.isExecutable( thisInterceptor, event, attributes.key ) ){
        // Invoke the execution point
        variables.invoker(
            interceptor     = thisInterceptor,
            event             = event,
            interceptData     = attributes.interceptData,
            interceptorKey     = attributes.key,
            buffer             = attributes.buffer
        );

        // Debug interceptions
        if( variables.log.canDebug() ){
            variables.log.debug( "Interceptor '#getMetadata( thisInterceptor ).name#' fired in asyncAll chain: '#this.getState()#'" );
        }
    }

} // end thread
```

## Thread Data

Because multiple threads can process simultaneously within a single template request, applications must ensure that data from one thread does not pollute or affect data in another thread. CFML provides several scopes that you can use to manage thread data, and a request-level lock mechanism that you use to prevent problems caused by threads that access page-level data. CFML also provides metadata variables that contain any thread-specific output and information about the thread, such as its status, processing time and much more, which extremely useful.

### Thread Scopes

* Thread `local` scope
* `Thread` scope
* `Attributes` scope

#### Thread local Scope

The thread-local scope is an implicit scope that contains variables that are available only to the thread, and exist only for the life of the thread. This exactly the same as the function `local` scope. Any variable that you define inside the `thread` body without specifying a scope name prefix is in the thread local scope and cannot be accessed or modified by other threads.

```java
thread name="mythread"{

    var localData = "this data"; // A thread local variable.
    believeItOrNot = "this goes into the local scope and not the variables scope";

}
```

#### `Thread` Scope

The `Thread` scope contains thread-specific variables and metadata about the thread. Only the owning thread can **write** data to this scope, but the page thread and all other threads in a request can **read** the variable values in this scope. Thread scope data remains available until the page and all threads that started from the page finish, even if the page finishes before the threads complete processing. **So be careful with what you store in this scope or you can create memory leaks.**

To write to this scope you can use the `thread` scope or actually the **name** of the thread as well, which is pretty cool.

To read from this scope outside the `thread` construct you can use the name of the thread or the thread scope, but you must reference which thread scope within it using it's name: `thread.myThreadName`

```java
thread name="mythread"{

    // create a shared struct data variable.
    thread.myDataProcess = {
        name = "hello"
    };
    thread.threadStartedAt = now();
    // Or use the thread name
    mythread.myDataProcess.value = 1;

}

// Outside of the thread, you can use the thread scope or the named thread.
cflog( thread.myThread.myDataProcess.hello );
cflog( mythread.myDataProcess.value + 1 );
```

{% hint style="danger" %}
Thread scoped variables are only available to the page that created the thread or to other threads created by that page. No other page can access the data, ever! If one page must access another page's `Thread scope` data, you must place the data in a shared location such as a shared scope or a file or database.
{% endhint %}

#### Thread Attributes Scope

The thread body executes in isolation, but sometimes you need to be able to pass certain data that the thread body cannot access. In this case, we will pass them via the `thread` construct as a name-value pair. The CFML engine will then place those in the thread's `attributes` scope so they can be used for the life of the thread.

**WARNING**

**All variables passed as attributes will be duplicated by the engine (deep copy). That's right! A raw duplicate() will be executed for the passing variable. If it is an array or struct, the entire collection will be duplicated. If it is an object with an object graph, the ENTIRE object graph will be duplicated.**

In some cases, that's ok, but it can be expensive and produce results that are not expected. Only pass variables to the attributes that you know and are ok with being duplicated. Copying the data ensures that the values passed to threads are thread-safe, because the attribute values cannot be changed by any other thread. If you do not want duplicate data, do not pass it to the thread as an attribute but use a shared scope instead that the thread body can access.

```java
// A fancy one
thread     name="#thisThreadName#"
        action="run"
        priority="#arguments.asyncPriority#"
        // custom variables passed as attributes
        interceptData="#arguments.interceptData#"
        threadName="#thisThreadName#"
        buffer="#arguments.buffer#"
        key="#key#"
{

    // Retrieve interceptor to fire and local context
    var thisInterceptor = this.getInterceptors().get( attributes.key );
    var event             = variables.controller.getRequestService().getContext();

    // Check if we can execute this Interceptor
    if( variables.isExecutable( thisInterceptor, event, attributes.key ) ){
        // Invoke the execution point
        variables.invoker(
            interceptor     = thisInterceptor,
            event             = event,
            interceptData     = attributes.interceptData,
            interceptorKey     = attributes.key,
            buffer             = attributes.buffer
        );

        // Debug interceptions
        if( variables.log.canDebug() ){
            variables.log.debug( "Interceptor '#getMetadata( thisInterceptor ).name#' fired in asyncAll chain: '#this.getState()#'" );
        }
    }

} // end thread
```

That's it for threading. Such a simple but powerful construct built right into the CFML language. Like mentioned before, if you need much more granular control or advanced ways to do [asynchronous programming](../beyond-the-100/asynchronous-programming.md), go to our section on running async code fluently.
