# Variables

In CFML, variables are just pointers to a piece of data.  They can hold **any** value you like and change it at runtime.  In some languages, you need to specify the type of data you want your variable to hold.  In CFML, you do not need to assign one as everything is dynamic.  The Lucee server even goes further and infers types according to the initial value you assign your variable.

A variable, like in math, is just a name for a piece of data. In CFML, variables are very flexible and can be changed at any time. Variables are assigned using a single equals sign `=` where the **right** side of the equals sign is evaluated first, then the value is assigned to the variable named on the **left** side of the equals.



Create a new CFML file called script_variables.cfm, enter in these example instructions, and observe the output that CFML gives you back:

#### Tag Syntax

```cfm
<cfoutput>
<cfset a = 5 />
a = #a#<br/>
<cfset b = 10 + 5 />
b = #b#<br/>
<cfset c = 15 + a + b />
c = #c#<br/>
<cfset b = c * a />
b = #b#<br/>
<cfset d = "Hello, " />
d = #d#<br/>
</cfoutput>
```

#### Script Syntax

```cfm
<cfscript>
a = 5;
writeOutput("a = #a#<br/>");
b = 10 + 5;
writeOutput("b = #b#<br/>");
c = 15 + a + b;
writeOutput("c = #c#<br/>");
b = c * a;
writeOutput("b = #b#<br/>");
d = "Hello, ";
writeOutput("d = #d#<br/>"); 
</cfscript>
```

In this second example, we assume the first example is present so don't create a new file but continue using script_variables.cfm.

#### Tag Syntax

```cfm
<cfoutput>
<cfset e = "World!" />
e = #e#<br/>
<cfset f = d & e />
f = #f#<br/>
<cfset g = d & a & e />
g = #g#<br/>
<cfset b = "hi!" />
b = #b#<br/>
</cfoutput>
```

#### Script Syntax

```cfm
<cfscript>
e = "World!";
writeOutput("e = #e#<br/>");
f = d & e;
writeOutput("f = #f#<br/>");
g = d & a & e;
writeOutput("g = #g#<br/>");
b = "hi!";
writeOutput("b = #b#<br/>");  
</cfscript>
```

*The first few lines in the first example are simple if you've done any programming language before, but the last few get interesting when combining strings and numbers. The code looks a little messy since after each instruction we output a variable.
