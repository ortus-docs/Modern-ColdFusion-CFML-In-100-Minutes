---
description: ColdFusion Markup Language <CFML> is a dynamic web programming language.
---

# What is ColdFusion (CFML)

ColdFusion Markup Language \<CFML> is a dynamic web programming language, which is especially suited for new developers as it was written to make a programmer's job easy and not care if the computer's job is hard. CFMLs primary goal is to be a rapid application development scripting language and middleware. It integrates with many technologies to provide an out-of-the-box language that makes things **easy**. This brief introduction will look at key language features you need to get started.

## Going Deep

![Lucee Server](../.gitbook/assets/lucee.png)

ColdFusion (CFML) is an interpreted and [dynamic ECMA Script like language](https://en.wikipedia.org/wiki/Dynamic\_programming\_language) that compiles to [Java Bytecode](https://en.wikipedia.org/wiki/Java\_bytecode) directly, thus running in the Java Virtual Machine (JVM) and in almost every operating system. Implementations of the language are mostly done by two parties: [Adobe ColdFusion](http://www.adobe.com/products/coldfusion-family.html) (Commercial) and [Lucee Server](http://lucee.org/) (Free & Open Source), and they saw their beginnings in 1995. It is a mature and modern language and development platform. You can discover all the versions here: [https://cfdocs.org/coldfusion-versions](https://cfdocs.org/coldfusion-versions)

![Adobe ColdFusion](../.gitbook/assets/acf.png)

## Developing with CFML

All examples in this book will leverage CommandBox as the de-facto standard for ColdFusion (CFML) development.

[CommandBox](https://www.ortussolutions.com/products/commandbox) is a standalone, native tool for Windows, Mac, and Linux that will provide you with a Command Line Interface (CLI) for developer productivity, tool interaction, package management, REPL, embedded ColdFusion/Java server, application scaffolding, and some sweet ASCII art.

## Docs Reference

The best way to discover the CFML language's methods, tags, and functionality is to leverage [cfdocs.org](https://cfdocs.org/). Make sure you open it and bookmark it.

{% embed url="https://cfdocs.org/" %}

## IDE - Editors

There are many flavors of IDE's but here are our recommendations, which all support CFML

* [Visual Studio Code](https://code.visualstudio.com/) _(Our Preference for both CFML and Java)_
  * Open-source packages
  * Adobe Package
* [Sublime](https://www.sublimetext.com/3)
* [Adobe ColdFusion Builder](http://www.adobe.com/products/coldfusion-builder.html)
  * Deprecated in favor of VSCode by Adobe
* [IntelliJ](https://www.jetbrains.com/idea/)

### **Sublime Packages**

Use [package control](https://packagecontrol.io/) in sublime to install the following packages which we use in our developer setups:

* ColdBox Sublime
* CommandBox Sublime
* Alignment
* CFML
* CFMLDocPlugin
* ColdFusion Docs Launcher
* DockBlockr
* Emmet
* Enhanced HTML and CFML
* SideBarEnhancements
* Terminal

{% hint style="success" %}
You can find the sublime package manager link here: [https://packagecontrol.io/](https://packagecontrol.io/)
{% endhint %}

### **VSCode Packages**

* Kamasamk CFML - [https://marketplace.visualstudio.com/items?itemName=KamasamaK.vscode-cfml](https://marketplace.visualstudio.com/items?itemName=KamasamaK.vscode-cfml)
* ColdBox Support - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-coldbox)
* CommandBox Support - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-commandbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-commandbox)
* TestBox Support - [https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-testbox](https://marketplace.visualstudio.com/items?itemName=ortus-solutions.vscode-testbox)
* Adobe ColdFusion Builder - [https://marketplace.visualstudio.com/items?itemName=com-adobe-coldfusion.adobe-cfml-lsp](https://marketplace.visualstudio.com/items?itemName=com-adobe-coldfusion.adobe-cfml-lsp)
* CFLSP - ColdFusion syntax error checker : [https://marketplace.visualstudio.com/items?itemName=DavidRogers.cflsp](https://marketplace.visualstudio.com/items?itemName=DavidRogers.cflsp)
* CFLint - Linting Support - [https://marketplace.visualstudio.com/items?itemName=KamasamaK.vscode-cflint](https://marketplace.visualstudio.com/items?itemName=KamasamaK.vscode-cflint)
* LuceeDebug - A barebones debugger - [https://marketplace.visualstudio.com/items?itemName=DavidRogers.luceedebug](https://marketplace.visualstudio.com/items?itemName=DavidRogers.luceedebug)
* Align
* Auto Alignment
* Auto CLose Tag
* Auto Rename Tag
* AutoFileName
* Better Comments
* CFGoto
* Code Outline
* Document This
* DotENV
* EditorConfig
* ESLint
* File Utils
* Git Graph
* Hibernate Log Analyser











{% hint style="info" %}
You can find the VSCode marketplace link here: [https://marketplace.visualstudio.com/VSCode](https://marketplace.visualstudio.com/VSCode)
{% endhint %}





