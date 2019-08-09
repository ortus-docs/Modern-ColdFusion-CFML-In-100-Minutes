# Threading

CFML allows you create asynchronous threads so you can execute a body of code in a separate thread.  This is achieved via the `cfthread` tag/construct.  Threads are independent streams of execution, and multiple threads on a page can execute simultaneously and asynchronously, letting you perform asynchronous processing in CFML. CFML code within the `cfthread` tag body executes on a separate thread while the page request thread continues processing without waiting for the `cfthread` body to finish.  You can allow the thread body to continue executing in the background or you can wait for it to finish.

This approach is very very simplistic, if you want more control of your asynchronous programming aspects then we can move into leveraging CFML Future's via the `runAsync()` function or parallel Java streams using the [cbStreams](https://www.forgebox.io/view/cbStreams) project. Please see our [Asynchronous Programming ](../beyond-the-100/asynchronous-programming.md)section for information on advanced asynchronous programming.

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/developing-cfml-applications/using-coldfusion-threads/creating-and-managing-coldfusion-threads.html" %}

{% embed url="https://helpx.adobe.com/coldfusion/developing-applications/developing-cfml-applications/using-coldfusion-threads/working-with-threads.html" %}

### A Fair Warning

Please note that once you get into concurrency you will start to get many headaches.  Your code must be thread safe, appropriate locking must be in place and overall concurrency based programming will be needed.  It is also very difficult to debug because you are no longer in the same thread and a `writedump() abort` combo usually goes into ether.  Logging will be your best friend and outputting logs to the console for debugging purposes.

## Thread Construct

Writing `thread` is extremely easy, just use the construct, give it a few attributes an boom you are in multi-threaded land.  In tags you can use the `<cfthread>` tag.

```java
thread
    name = "The name of the thread, must be unique"
    action="run, sleep, join, or terminate"
    duration="The number of milliseconds to suspend the thread processing"
    priority="high, low, or normal"
    timeout="Number in milliseconds the current thread waits for this thread to finish."
{
    // Body to execute in a separate thread
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
thread 	name="#thisThreadName#"
		action="run"
		priority="#arguments.asyncPriority#"
		interceptData="#arguments.interceptData#"
		threadName="#thisThreadName#"
		buffer="#arguments.buffer#"
		key="#key#"
{

	// Retrieve interceptor to fire and local context
	var thisInterceptor = this.getInterceptors().get( attributes.key );
	var event 			= variables.controller.getRequestService().getContext();

	// Check if we can execute this Interceptor
	if( variables.isExecutable( thisInterceptor, event, attributes.key ) ){
		// Invoke the execution point
		variables.invoker(
			interceptor 	= thisInterceptor,
			event 			= event,
			interceptData 	= attributes.interceptData,
			interceptorKey 	= attributes.key,
			buffer 			= attributes.buffer
		);

		// Debug interceptions
		if( variables.log.canDebug() ){
			variables.log.debug( "Interceptor '#getMetadata( thisInterceptor ).name#' fired in asyncAll chain: '#this.getState()#'" );
		}
	}

} // end thread
```

## Thread Data

Because multiple threads can process simultaneously within a single request, applications must ensure that data from one thread does not improperly affect data in another thread. CFML provides several scopes that you can use to manage thread data, and a request-level lock mechanism that you use to prevent problems caused by threads that access page-level data. CFML also provides metadata variables that contain any thread-specific output and information about the thread, such as its status,  processing time and much more.

### Thread Scopes

* Thread `local` scope
* `Thread` scope
* `Attributes` scope

#### Thread local Scope

The thread-local scope is an implicit scope that contains variables that are available only to the thread, and exist only for the life of the thread.  This exactly the same as the function `local` scope. Any variable that you define inside the `thread` body without specifying a scope name prefix is in the thread local scope and cannot be accessed or modified by other threads.

```java
thread name="mythread"{
    
    var localData = "this data"; // A thread local variable.
    believeItOrNot = "this also goes into the local scope";

}
```

#### `Thread` Scope

The `Thread` scope contains thread-specific variables and metadata about the thread. Only the owning thread can **write** data to this scope, but the page thread and all other threads in a request can **read** the variable values in this scope. Thread scope data remains available until the page and all threads that started from the page finish, even if the page finishes before the threads complete processing. So be careful with what you store in this scope.

To write to this scope you can use the thread scope or actually the name of the thread as well.

```java
thread name="mythread"{
    
    thread.myDataProcess = {
        name = "hello"
    };
    // Or use the thread name
    mythread.myDataProcess.value = 1;

}

cflog( thread.myDataProcess.hello );
cflog( mythread.myDataProcess.value + 1 );
```

