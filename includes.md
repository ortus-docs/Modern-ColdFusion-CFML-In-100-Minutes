# Includes

If you've used other scripting environments such as PHP, or dynamic HTML, you would be familiar with the concept of **server side includes**.  An **include** is a file that is embedded, or **included** within another file; simple as that. 

This can be very useful when you want multiple CFML templates to share the same block of code, scopes and visibility.  In modern times, you can call this a **mixin**. 

A typical example might be your website's header and footer, or the reuse of included functions.  The ColdBox MVC framework even allows you to define mixin helper templates that can be injected at runtime in Controller objects, views, layouts and much more.

Now, even though doing includes/mixins are easy to do in CFML, let me give you a **BIG WARNING**.  Includes are one of the most abused features in ANY language.  Easy doesn't mean sustainable or maintainable.  Do not go crazy with includes, there are many other design patterns like dependency injection and composition/aggregation that can solve reusability in much better approaches.

> **Mixin** : In object-oriented programming languages, a mixin is a class that contains methods for use by other classes without having to be the parent class of those other classes; No inheritance needed. - https://en.wikipedia.org/wiki/Mixin

## Implementation

CFML provides the `<cfinclude>` tag and the `include` construct for including files in script - https://cfdocs.org/cfinclude.

Tag Syntax

You'll notice that the <cfinclude> tag doesn't have a closing tag — it just accepts an attribute value (the included filename).


<cfinclude template="included_filename">
Code Example

A common use of ColdFusion includes is to output a consistent header and footer throughout each page of a website. In this example, we're going to create a basic ColdFusion template which uses the cfinclude tag to include two other files.

File 1: index.cfm


<cfinclude template="header.cfm">
​
<p>Welcome to my website on ColdFusion cfinclude usage!</p>
​
<cfinclude template="footer.cfm">
File 2: header.cfm


<h1>This is the header</h1>
File 3: footer.cfm


<h1>This is the footer</h1>
Once you've created all of the above files, open index.cfm in your browser. The page should contain the contents of header.cfmat the top, and footer.cfm at the bottom.

Now create a fourth ColdFusion template as follows and open it in your browser.

File 4: about.cfm


<cfinclude template="header.cfm">
​
<p>Who are we? We are ColdFusion cfinclude enthusiasts of course!</p>
​
<cfinclude template="footer.cfm">
Because this template includes the same header and footer file, you will see the same header and footer when displayed in a browser.