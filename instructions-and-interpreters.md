# Language Introduction

ColdFusion is an **interpreted** programming language which means it can’t run on your processor directly, it has to be fed into a middleman called the Java Virtual Machine in the form of Java Bytecode. It is also a **dynamic language**, meaning you do not have the **typed** restrictions a compile-time language like Java has.  This means you have greater flexibility as the engine actually infers the types for you (Lucee) and it allows you to do runtime manipulations like method injections, removals, metadata programming etc that a typical typed language would not allow.  It also allows us to not be in the dreaded compile, build, deploy cycle, since the ColdFusion scripts will be evaluated, compiled and executed all at runtime. No need of re-deploying or annoying restarts.

However, with much power comes greater responsibility.  There is a lot more potential of runtime exceptions due to the fact that these exceptions cannot be caught by a compiler at compilation time, as compilation occurs at the same time as execution.  Thus, unit and integration testing become a real asset when building applications under a dynamic language.

The ColdFusion engine will convert your markup into byte code and feed it into the Virtual Machine (VM) to execute it.  The benefit to this approach is that you can write ColdFusion code once and, typically, execute it on many different operating systems and hardware platforms.

You can run any ColdFusion script in any Adobe or Lucee server or in the command line with CommandBox.

> Running via CommandBox in the command line will leverage the Lucee CFML engine.

## Running ColdFusion from the Command Line

This is the durable way to write ColdFusion code because you save your instructions into a file. That file can then be backed up, transferred, added to source control, etc.

### An Example ColdFusion File

We might create a file named `hello.cfm` like this:

```js
<cfoutput>Hello from CFML Land!</cfoutput>
```

Then we could run the program like this `box hello.cfm` and get the following result:

```
Hello from CFML Land!
```

> When you run `box hello.cfm` you’re actually loading the CFML instruction set engine (Lucee) and executing the code.  Please note, there is NO web server here. It is a pure command line execution.

## CommandBox REPL

CommandBox sports a ColdFusion Read Eval Print Loop interface or most commonly know as REPL.  The REPL is like a programming calculator, input in output out.  It will execute ColdFusion instructions and give you feedback on syntax and results.  To start a REPL we must go into the CommandBox shell by typing just `box` or opening the `box` binary.

Once in the CommandBox prompt type `repl` and you will be placed in REPL mode:

<img src="assets/repl.png" alt="CommandBox" />

Please note that the REPL in CommandBox opens in **script** mode and not in **tag** mode.  This means that we must type in instructions that adhere to the ColdFusion scripting or ECMA script like syntax instead of the tag based syntax.  We will discover more about syntax in the next chapter.

For now, let's type the equivalent in Script syntax:

```js
writeOutput( "Hello from CFML Land!" )
```

<img src="assets/repl-hello.png" alt="CommandBox" />

Boom!  We get a magical hello from the CommandBox REPL.



