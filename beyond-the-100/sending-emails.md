# Sending Emails

CFML gives you the `cfmail` tag/construct to easily send email in text or HTML format without any ceremony.  Just register your mail servers in the administrators or the `Application.cfc` and you are ready to start sending emails in a jiffy!

```java
cfmail( 
  subject="Your Order", 
  from="myshop@ortus.com", 
  to="whatever@gmail.com,another@gmail.com",
  bcc="orders@ortus.com"
  type="HTML"
){
  // body of the email.
  writeOutput( 'Hi there,' );
  writeOutput( 'This mail is sent to confirm that we have received your order.' );
};
```

The `cfmail` tag/construct has tons of attributes, so check them out in the docs [https://cfdocs.org/cfmail](https://cfdocs.org/cfmail)

### Query Binding

You can even bind the mail construct with a query, and the engine will send as many emails as rows in the query for you:

```java
var qData = userService.getNewUsers();
cfmail( 
  subject="Welcome to FORGEBOX!", 
  from="myshop@ortus.com", 
  to="#qData.email#",
  query=qData
){
  writeOutput( "
    Dear #qData.name#,
    
    Welcome to your FORGEBOX account! Play and just do it!
  ")
};
```

### Sending Attachments

You can also send attachments to your email destinations very easily using the `mimeattach` attribute or via the child `cfmailparam()` construct, which allows you to send multiple attachments, or headers.

```java
cfmail( 
  subject="Your Order", 
  from="myshop@ortus.com", 
  to="whatever@gmail.com,another@gmail.com",
  bcc="orders@ortus.com"
  mimeattach=expandPath( "/my/path/attach.pdf" );
){
  // body of the email.
  writeOutput( 'Hi there,' );
  writeOutput( 'This mail is sent to confirm that we have received your order.' );
};

cfmail( subject="Attachments", to="you@domain.com", from="me@domain.com" ) {
	cfmailparam( name="Reply-To", value="me@domain.com" );
	cfmailparam( file="c:\files\readme.txt" );
	cfmailparam( file="c:\files\logo.gif" );
}
```

{% hint style="success" %}
More in-depth information can be found here: [https://helpx.adobe.com/coldfusion/developing-applications/using-external-resources/sending-and-receiving-e-mail/sending-e-mail-messages.html](https://helpx.adobe.com/coldfusion/developing-applications/using-external-resources/sending-and-receiving-e-mail/sending-e-mail-messages.html)
{% endhint %}



