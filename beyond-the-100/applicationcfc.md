# Application.cfc

## Introduction

If you are running ColdFusion in a [web server like CommandBox](https://commandbox.ortusbooks.com/embedded-server) and a ColdFusion page (.cfm/.cfc)  is requested, the engine will look for a special file called `Application.cfc` and if found, it will execute it for you **implicitly**.  This file is used to define the following:

1. Application-wide settings, default variables, session/client storages, file mappings, datasources, style settings, ORM settings and so much more. These are created by placing them in the `this` scope and in the pseudo-constructor of the object.
2. Application **life-cycle** event handlers which the engine will execute for you **implicitly** by you just creating the appropriate available methods.

{% hint style="info" %}
Usually you will place this file in your **webroot**.  Any page in the same directory or sub-directories that is requested will execute this file implicitly.
{% endhint %}

### Transient File

This file is created on every request, so make sure that it is optimized accordingly.  The `application` scope can be leveraged for global persistence.  Also note, that because it is instantiated on every request, you have the potential to change settings and events on a per-request basis if needed.  Great use cases for this can be the following:

* Switching datasources
* Lowering session/client timeouts for scopes if a bot is requesting a page
* Increased security
* Stopping requests
* Much More...

{% hint style="warning" %}
Please note that this file is execute for every request, so make sure it is optimized accordingly.  The engine will also traverse your directories upwards until it finds this file.
{% endhint %}

### Sample Template

{% code title="Application.cfc" %}
```javascript
component{

    this.name = "My Awesome App";
    this.applicationTimeout = createTimeSpan( 30, 0, 0, 0 ); //30 days
    this.sessionStorage = true;
    this.sessionTimeout = createTimeSpan( 0, 0, 60, 0 ); // 1 hour
    
    function onApplicationStart(){}
    function onApplicationEnd( struct applicationScope ) {}
    
    function onSessionStart() {}
    function onSessionEnd( struct sessionScope, struct applicationScope ) {}
    
    function onRequestStart( string targetPage ) {}
    function onRequest( string targetPage ) {
        include arguments.targetPage;
    }
    function onRequestEnd() {}
    function onCFCRequest( cfcname, method, struct args) { 
        return;
    } 
    
    function onError( any Exception, string EventName ) {}
    function onAbort( required string targetPage ) {} 
    function onMissingTemplate( required string targetPage ) {}

}
```
{% endcode %}

{% hint style="info" %}
You can find all the settings and event handlers defined for `Application.cfc` in cfdocs: [https://cfdocs.org/application-cfc](https://cfdocs.org/application-cfc).&#x20;
{% endhint %}

## Life-Cycle Events

The `Application.cfc` also acts like a big event listener waiting for the ColdFusion server engine to call its methods in callback fashion.  You can listen to when an error occurs to even when a missing template is requested and much more.  If I am missing some here, please refer to the latest documentation for the latest updates: [https://cfdocs.org/application](https://cfdocs.org/application)

* **onApplicationStart**: The very first time your application is requested, the `onApplicationStart` event is broadcast. It only happens once, until your application times out, the process is restarted, or the computer is restarted.
* **onSessionStart**: Whenever a NEW user requests any resource in your web application ColdFusion will assign them a unique session identifier and call this method for you.
* **onRequestStart**: Executes before every requested resource and receives the targeted page in the arguments.  You can decide to process the page or not by returning a boolean variable.
* **onRequest**: This callback allows you to actually include the requested page or not. Consider the `onRequestStart()` as a before advice and the `onRequest` as an around advice over the request. You must `include` the requested page or the page will never be processed
* **onRequestEnd**: Also receives the targetPage and executes after onRequestStart() and onRequest() have fired.
* **onSessionEnd**: Once a unique session has expired this method will be called for you by the engine with the session and application scope structure as arguments.
* **onApplicationEnd**: Once the application times out or is flushed you can listen to it. It also accepts the entire application scope as an incoming argument.
* **onError**: Like a big try/catch statement across the application.  This will fire whenever an untrapped exception is caught.  You will receive the exception struct and the `eventName` that generated the exception.
* **onAbort:** Runs when you execute the abort tag somewhere and you want to know where that pesky abort exists. It receives the `targetPage` that the abort came from.
* **onCFCRequest**: Intercepts any HTTP or AMF calls to an application based on CFC request.
*   **onMissingTemplate**: Executed whenever a template is requested and it does not exist in the server.



{% hint style="info" %}
The [ColdBox HMVC Framework](https://www.coldbox.org) builds heavily on top of these life-cycle methods to provide you with a rich event-driven architecture for web applications. You can create an application with CommandBox just by running: `coldbox create app MyApp`
{% endhint %}

## ColdFusion Web Applications

ColdFusion differs from other Java web languages in the sense that there is one Java context application deployed (The CFML Engine) with many servlet definitions, but you can create many ColdFusion applications within the running context.  All you need is to demarcate them with the `Application.cfc`, with a unique name which defines the separate ColdFusion applications.  **Anything in that directory and sub-directories will be considered part of the application.**

Your ColdFusion application is nothing more than a memory space reservation using the `this.name` property as the unique name for it.  It can contain the variables scopes like `application`, `session`, and `client` which are unique per this reservation name. This way two ColdFusion applications can have different persistence variable scopes and can even be embedded between each other:

```
+ webroot
  + admin/
     + Application.cfc // Embedded admin app
  + Application.cfc // Root public app
```

### Application Longevity

This is why you can't really kill the `application` scopes. The application is reserved for a specific duration which is defined using the `this.applicationTimeout` property or defined in the Administrator. Once it expires, the engine will purge it from memory and recreate it fresh.

{% hint style="success" %}
**Tip** You can force the stopping of the Application scope by leveraging the method `applicationStop()`.  Be careful though, as it stops full execution and restarts it. ([https://cfdocs.org/applicationstop](https://cfdocs.org/applicationstop))
{% endhint %}



