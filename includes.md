# Includes

If you've used other scripting environments such as PHP, or dynamic HTML, you would be familiar with the concept of **server side includes**.

An **include** is a file that is embedded, or **included** within another file; simple as that. This can be very useful when you want multiple ColdFusion templates to share the same block of code. A typical example might be your website's header and footer. If your website has a consistent header and footer on every page, you could use an include file for each of these.

ColdFusion provides the <cfinclude> tag for specifying included files.

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