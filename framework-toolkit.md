---
title: 'Library - Framework and Tookit'
subtitle: 'Shareable Code aspects'
image: https://miro.medium.com/max/5600/1*rujr0xAqANInP9pdzXZcsw.png
datePublished: '2018-01-25'
---

These days software applications are complex in their functions. It is an arduous task to build an application on top of all own handwritten code, and it is not advisable.

It is in practice to build applications by combining owned code with shared code to match up with the fast pace of world demand of software.

Code is distributed/shared as **Libraries** _(tangible unit)_.

## Library

A _Library_ can be a single archive file or a set of files. Language Specification / OS platform / Package Manager specifies the distribution style of a library.

### Practical Examples:

- Java ecosystem uses language standard defined layout for shared code in the archived file so-called .jar ("jar" - Java Archive).
- C/C++ uses OS platform based file `.so` (shared object) on Unix and `.dll` (dynamic link library) on Windows.
- JavaScript (Node Ecosystem) uses a directory path based mechanism for shared code.

_Frameworks and Toolkits are aspects of Libraries_.

## Framework

A _framework_ enforces you to write code in a set of code chunks (framework elements) and calls your engineered code-chunks to do its business. **You are never in-charge of calling these code chunks.**

In essence, a Framework enforces to develop a mindset to write application code in _term or base_ of the framework. Might be, this is the reason for the name "framework".

Typically, you annotate or mark your code elements (classes, objects, method) with framework elements. Marking process may vary language to language.

> In languages like Python, Ruby, JavaScript, You mark code by inheriting classes from framework-defined classes.

> In languages like Java, you can mark your code by extending classes or by marking classes or methods with Java Annotations (see example below).

Some frameworks are based on configuration descriptors. Usually, these configuration descriptors are written in text files in standard format (JSON, YAML, XML, TOML) or custom text format.

Generally, A Framework follows best practices or design practices.

---

### Framework Example

[Hibernate](https://hibernate.org/) (Java) is an Object Relational Mapping framework¹ (a technical style framework). You have to write entities, their relationships and DB configuration only in a declarative fashion.
You annotate a class with `@Entity` to make it an entity.

```java
@Entity
class Student {
  @Id
  private long id;
}
```

- **Technical Level Frameworks** are focused on providing technical problems solutions. They are smaller in scope, thus permits you an option to combine with multiple frameworks to write an application.
  Typical Examples

* Transactions ([Spring](https://spring.io/) [Java]),
* Object Relational Mapping ([Hibernate](https://hibernate.org/)[Java], Sequelize [Node.js] ..)
* Dependency Injection ([Dagger](https://dagger.dev/), Spring IOC, Guice/Java)
* [Angular](https://angular.io/), [React](https://reactjs.org/), [Vue](https://vuejs.org/) (Fronted frameworks)

![frameworks](https://miro.medium.com/max/5600/1*rujr0xAqANInP9pdzXZcsw.png)

- **Domain-Level Frameworks** are focused on business or domain level solutions. These are broader in scope than technical level frameworks. 
  Usually, these are termed as "platform".
  e.g. [Tuya](https://en.tuya.com/) is IoT platform

- **Opinionated Framework** implements or enforces you to implement a certain kind of practices. 
  If domain level then it compels to implement industry practices. 
  If technical level then it enforces best practices of development.

- **Non-opinionated Framework** - Probably, you guess it. These do not bind you to any practice. You are free to implement your way of doing things. Example: Express (JS@Node)

## Toolkit

In simple words, a Toolkit is just a set of functions or code elements.
Your application code calls these functions or elements. In other words, You are in full control of calling these functions. Typically, you combine multiple toolkits for writing an application.

### Toolkit Example 

[jQuery](https://jquery.com/) is a web library which provides DOM manipulation utilities.

```javascript
// Application Code calls library code
$('#welcome').html('<b>hello world</b>')
```

Toolkits are generally written for:

- Text or binary format parsing: XML, JSON, CSV, YAML, XLS, PDF…
- Date and Time parsing, formatting, manipulation
- Data Structure and utilities
- Pooling: Connection, Thread
- Protocol clients: HTTP, SMTP, FTP, MQ …

---

## Takeaway

- Code is shared in the form of libraries.
- A library can be Framework or Toolkit.
- A Framework enforces you write code in its term.
- Frameworks call your annotated or marked code.
- A toolkit is a set of functions.
- Your code calls functions of a toolkit.

---

## Foot Notes

1. **Object Relationship Mapping (ORM)** Framework bridges gaps within DB model (concrete / physical model) and business entity (abstract /logical model).
