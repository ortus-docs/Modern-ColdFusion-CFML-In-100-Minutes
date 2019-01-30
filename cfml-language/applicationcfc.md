# Application.cfc

## Introduction

If you are running ColdFusion in a [web server like CommandBox](https://commandbox.ortusbooks.com/embedded-server) and a ColdFusion page \(.cfm/.cfc\)  is requested, the engine will look for a special file called `Application.cfc` and if found, it will execute it for you **implicitly**.  This file is used to define the following:

1. Application-wide settings, default variables, session/client storages, file mappings, datasources, style settings, ORM settings and so much more. These are created by placing them in the `this` scope and in the pseudo-constructor of the object.
2. Application life-cycle event handlers which the engine will execute for you **implicitly** by you just creating the appropriate available methods.

{% hint style="info" %}
Usually you will place this file in your webroot.  Any page in the same directory or sub-directories that is requested will execute this file implicitly.
{% endhint %}

### Transient File

This file is created on every request, so make sure that it is optimized accordingly.  The `application` scope can be leveraged for global persistence.  Also note, that because it is instantiated on every request, you have the potential to change settings and events on a per-request basis if needed.  Great use cases for this can be the following:

* Switching datasources
* Lowering timeouts for scopes if a bot is requesting a page
* Increased security
* Stopping requests
* Much More...

{% hint style="warning" %}
Please note that this file is execute for every request, so make sure it is optimized accordingly.  The engine will also traverse your directories upwards until it finds this file.
{% endhint %}

### Sample Template

{% code-tabs %}
{% code-tabs-item title="Application.cfc" %}
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
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
You can find all the settings and event handlers defined for `Application.cfc` in cfdocs: [https://cfdocs.org/application-cfc](https://cfdocs.org/application-cfc). 
{% endhint %}

## ColdFusion Web Applications

ColdFusion differs from other Java web languages in the sense that there is one Java context application deployed \(The CFML Engine\) with many servlet definitions, but you can create many ColdFusion applications within the running context.  All you need is to demarcate them with the `Application.cfc`, with a unique name which defines the separate ColdFusion applications.  **Anything in that directory and sub-directories will be considered part of the application.**

Your ColdFusion application is nothing more but a memory space reservation using the `this.name` property as the unique name for it.  It can contain the variables scopes like `application`, `session`, and `client` which are unique per this reservation name. This way two ColdFusion applications can have different persistence variable scopes and can even be embedded between each other:

```text
+ webroot
  + admin/
     + Application.cfc // Embedded admin app
  + Application.cfc // Root public app
```

### Application Longevity

This is why you can't really kill the `application` scopes. The application is reserved for a specific duration which is defined using the `this.applicationTimeout` property or defined in the Administrator. Once it expires, the engine will purge it from memory and recreate it fresh.

{% hint style="info" %}
You can force this by leveraging the method `applicationStop()`.  Be careful though, as it stops full execution and restarts it. \([https://cfdocs.org/applicationstop](https://cfdocs.org/applicationstop)\)
{% endhint %}





