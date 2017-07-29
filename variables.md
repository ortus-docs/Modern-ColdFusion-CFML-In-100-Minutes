# Variables

In CFML, variables are just pointers to a piece of data.  They can hold **any** value you like and change it at runtime.  In some languages, you need to specify the type of data you want your variable to hold.  In CFML, you do not need to assign one as everything is dynamic.  The Lucee server even goes further and infers types according to the initial value you assign your variable.

Please note that assignments are evaluated from right to left instead of traditional reading which is from left to right.

Open up the CommandBox Shell and go into **REPL** mode by typing `repl`.  Every time you assign a value to a variable, the CommandBox REPL will output or echo the variable for you.

![](/assets/variables.png)

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
