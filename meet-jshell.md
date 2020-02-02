---
title: Meet JShell - Java REPL tool
subtitle: 'Java Fiddler'
description: Jshell is a new tool in Java ecosystem to quickly play or experiment with Java code.
keywords: 'java, jshell, jshell variables, jshell save session, jshell exit, jshell start, how to start jshell, jshell help, jshell edit'
image: https://cdn-images-1.medium.com/max/1600/1*VtOc8W-xR2iy8hHRUkZZJw.png
datePublished: '2017-09-03'
lastModified: '2020-02-02'
tags: 'java, tutorial'
---

JShell is one of the new addition to JDK system since Java 9.

## REPL

**R**ead **E**val **P**rint **L**oop is an interactive tool. Typically, It is command-line based that _reads_ a statement or an expression or a definition _evaluates_ them, _prints_ the result or prints the error message if there is any problem in execution and keep on looping the same until you asked it to quit.

The name REPL is adopted from LISP language where the construct of reading, evaluating, printing as:

```lisp
(print (eval (read)))
```

REPL is an easy and quick way to experiment or fiddle with a small piece of code or try out features of language or checking the result of an expression without going through the sophistication of files or IDE project.

_It is not a new kind of tool_. Command Prompt or Shell is an excellent example of this type of tool. Also, many language ecosystems already support REPL: JavaScript (Node ecosystem and within Browser), Python, Ruby…, now Java is in the league.

## JShell

JShell _(Java Shell)_ is the name of Java REPL tool. It supports two types of constructs:

1. Command: Commands are special instructions to the JShell. A command in JShell starts with starting with `/`.   
   e.g.
   `/help` or `/?` to know about JShell and its commands.  
   `/exit` to exit from the shell.
2. Java Snippet: A Java language code piece like statement, expression, definition

### Real Meat 🍖

JDK 9 or later distribution contains a program named `jshell`. If `$JDK/bin`
is in the path, then JShell can be activated by typing `jshell`.

```sh
$ jshell
| Welcome to JShell -- Version 9
| For an introduction type: /help intro
jshell>
```

JShell starts with a welcome message along with prompt `jshell>` which indicates that it is ready for accepting input from you.

### Statements & Expressions

Type any statement or expression to get started with it. You would be astonished after seeing the immediate result.

```java
jshell> 2 + 3 // expression
$1 ==> 5        // Result is shown as soon as you finished
jshell> int x = 10 // variable | ; is optional
x ==> 10
jshell> new Date()
$3 ==> Mon Aug 28 10:21:21 EST 2017
```

A statement should not end with `;`, but if you are clubbing multiple statements in a single line, then it is mandatory to insert ; in between them (at least).

### Variables

```java
jshell> int y = 1;double z = 2.0
y ==> 1
z ==> 2.0
```

To know the value of a variable, you can type the variable name.

JShell assigns each expression a name starting with `$` and followed by a number only if you have not assigned the name for it, e.g. `new Date()` is assigned `$3`.

```java
jshell> x
x ==> 10
jshell>$3
$3 ==> Mon Aug 28 10:21:21 IST 2017
```

If you could not remember what name JShell has assigned to your expression, don't worry; just type `/vars` and voila! It lists all the variables you have typed and expression values.

```java
jshell> /vars
| int $1 = 5
|    int x = 0
|    Date $3 = Mon Aug 28 10:21:21 IST 2017
| int y = 1
| double z = 2.0
```

> A lot can happen over a session of JShell.

### Control Statements, methods, types, …

```java
jshell> for (int hi = 0; hi < 2; hi++) {
...> System.out.println("hi"); //; is required here.
...> }
hi
hi
jshell> char grade(double number) {
...> if (number > 90) return 'A';
...> if (number > 80) return 'B';
...> if (number > 70) return 'C';
...> if (number > 60) return 'D';
...> if (number > 50) return 'E';
...> return 'F';
...> }
| created method grade(double)
jshell> grade(2)
$10 ==> 'F'
jshell> class A {
...> }
|  created class A
```

Just like `/vars`, there are more listing commands:  
`/method` lists all the methods defined.  
`/types` lists all the types defined.  
`/import` lists all the imports.

### Loading External Jar

> Don't limit yourself to Java inbuilt wall, when you can do endless by jumping the wall.

An external jar can be loaded with the following commands:  
`/reload` -class-path <path-to-jar>   
`/env` -class-path <path-to-jar>

```java
jshell> /reload -class-path ~/Dev/Repo/maven/commons-lang/commons-lang/2.6/commons-lang-2.6.jar
jshell> import org.apache.commons.lang.StringUtils;
jshell> StringUtils.isEmpty("")
\$11 ==> true
```

### Code Completion

JShell does not feature Intellisense; however, it presents a very close feature commonly known as _Code Completion_. When in doubt, press <kbd>tab</kbd> to get the all the alternatives or let it complete the method name or code at the cursor.

Code completion feature gives the bonus of JavaDoc when it is asked for immediately after left open brace `(` in case of a method.

```java
jshell> new Date( // when tab is pressed
Signatures:
Date()
Date(long date)
Date(int year, int month, int date)
Date(int year, int month, int date, int hrs, int min)
Date(int year, int month, int date, int hrs, int min, int sec)
Date(String s)
<press tab again to see documentation>

jshell> new Date( // on pressing tab from previous point
Date()
Allocates a Date object and initializes it so that it represents the time at which it was allocated, measured to the nearest millisecond.
<press tab to see next documentation>
```

### Working with files

It is possible to store the content of the session to a file.  
`/save <path-to-file>` stores only Java Snippets.  
`/save -history <path-to-file>` stores Java Snippets and JShell commands.

e.g.  
`/save ~/dev/jshell.jsh` stores the following content to a file named jshell.jsh.

```java
2+3
int x = 10;
new Date()
int y = 1;
double z = 2.0;
for (int hi = 0; hi < 2; hi++) {
System.out.println("hi");
}
char grade(double number) {
if (number > 90) return 'A';
if (number > 80) return 'B';
if (number > 70) return 'C';
if (number > 60) return 'D';
if (number > 50) return 'E';
return 'F';
}
grade(2)
class A{
}
import org.apache.commons.lang.StringUtils;
StringUtils.isEmpty("")
```

`/save -history ~/dev/jshell-with-commands.jsh` stores Java snippets along with JShell commands.

```java
2+3
int x = 10
new Date()
int y = 1;double z = 2.0
x
\$3
/vars
for (int hi = 0; hi < 2; hi++) {
System.out.println("hi");
}
char grade(double number) {
if (number > 90) return 'A';
if (number > 80) return 'B';
if (number > 70) return 'C';
if (number > 60) return 'D';
if (number > 50) return 'E';
return 'F';
}
grade(2)
class A {
}
/reload -class-path ~/Dev/Repo/maven/commons-lang/commons-lang/2.6/commons-lang-2.6.jar
import org.apache.commons.lang.StringUtils;
StringUtils.isEmpty("")
```

> If it is possible to save, then it is also possible to open or load the content of the same?

Hell! You are thinking right.
`/open <file-path>` loads the content of the file specified by <file-path>.

### A word of wisdom

If you load `~/dev/jshell.jsh` in a different session, the statement `import org.apache.commons.lang.StringUtils;`would throw error but with `~/dev/jshell-with-commands.jsh`, there would not be any error thrown.

With `jshell-with-commands.jsh`, `/reload` command would rerun and loads the jar.

With the `jshell.jsh`, there is no `/reload` command to load the jar.
So, **it is a sign of wisdom** if you are dealing with external jars in JShell session, then save the file with `/save -history`.

### Editing Snippet

JShell tags an **Id**entifier each and every snippet. `/list` lists all the snippets of the current session.

```java
jshell> /list
1 : 2 + 3
2 : int x = 10;
3 : new Date()
4 : int y = 1;
5 : double z = 2.0;
6 : for (int hi = 0; hi < 2; hi++) {
System.out.println("hi");
}
7 : char grade(double number) {
if (number > 90) return 'A';
if (number > 80) return 'B';
if (number > 70) return 'C';
if (number > 60) return 'D';
if (number > 50) return 'E';
return 'F';
}
8 : grade(2)
9 : class A {
}
10 : import org.apache.commons.lang.StringUtils;
11 : StringUtils.isEmpty("")
```

What if you want to change the value of the x *(tagged by id 2)* ?   
What if you want to change the definition of A class *(tagged by 9)* ?

Yes, All these are possible with `/edit` command. `/edit` opens up a GUI utility named **Edit Pad** which is a basic text editor. It accepts id or name of the definition as an argument.

e.g.  
`/edit 9` or `/edit A` opens up as shown below:

![editpad](/post/jshell.png)

There is one more way to edit a definition by **redefining** it. Redefining may have raised your eyebrows, despite it is a legitimate in JShell.

```java
jshell> class A {
...> private int a;
...> A(int a) {
...> this.a = a;
...> }
...> }
| replaced class A
```

### Asking for Help

JShell provides a helpline to get the help about it, all you have to dial `/help`. It lists all the commands with brief usage.
To get more details about a command

```sh
/help <command>
```

```
jshell> /help /exit
|
| /exit
|
| Leave the jshell tool. No work is saved.
| Save any work before using this command
```

If documentation is too geeky for you, don't be shy to ask for the help.

### Exiting from the shell

`/exit` command helps you to get out of JShell.

```
jshell> /exit
| Goodbye
```

### Still hungry?

Read the following bibles:

1. [JEP 222](http://openjdk.java.net/jeps/222)
2. [JShell Reference](https://docs.oracle.com/javase/9/tools/jshell.htm)
