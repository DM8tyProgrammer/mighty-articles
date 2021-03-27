---
title: What is Parsing?
subtitle: A brief introduction
description: Parsing is a process of converting formatted text into a data structure.
keywords: parsing, scanning, tokenisation, parsing phases, parsing example
datePublished: ‘2019-08-05’
lastModified: ‘2020-12-19’
image: https://miro.medium.com/max/1400/1*AmS9rVgJeizwqtC0_XHGlw.png
---

**Parsing** is a process of converting _formatted text_ into a _data structure_. A data structure type can be any suitable representation of the information engraved in the _source text_.

- _Tree_ type is a common and standard choice for XML parsing, HTML parsing, JSON parsing, and any programming language parsing. The output tree is called _Parse Tree_ or _Abstract_ _Syntax Tree_. In HTML context, it is called _Document Object Model_ (DOM).
- A CSV file parsing can result in a List of List of values _or_ a List of Record objects.
- _Graph_ Type is a choice for natural language parsing.

A piece of program that does parsing is called _Parser_.

## How it works
Parser analyses source text against the format* prescribed. If source text does not match against format error is thrown or returned.
* If source text does not match against format error is thrown or returned.
* If matches then “data structure” is returned.

![](https://cdn-images-1.medium.com/max/1600/1*Tj1H3orLHUpQlbBR45iNNA.png)

*format is coded inside the parser.
> Format is the DNA of a parser.

## Small Case Study

Consider an example of Date parsing from a string (source) in format `DD-MM-YYYY` to Date object:

```java
class Date {
  int day;
  int month;
  int year;
}
```

![](https://miro.medium.com/max/1400/1*AmS9rVgJeizwqtC0_XHGlw.png)

### Implementation Note

For Date parsing, I would be using *Regular Expression*¹ (_regex_ for short). Regex can be matched against a string. It also helps in extracting part of the source text _if it is matched._


_Note: This is going to be a small-scale illustration of parsing with "Regex." This might be a fair approach for one-two lines input text but not in every case. You may have to write a parser by hand (most complex) or use Parser Generator tools (moderately complex)._

### Code

The parsing and extracting date element is as:

```java
String date = “20-05-2012”;
// 1. defining a regular expression
Pattern dateMatcher = Pattern.compile(“(\\d{2})-(\\d{2})-(\\d{4})”);
// 2. matching a regular expression
Matcher matcher = dateMatcher.matcher(date);
// 3. if pattern matches
if (matcher.find()) {
  // extract day
  int day = Integer.parseInt(matcher.group(1));
  int month = Integer.parseInt(matcher.group(2));
  int year = Integer.parseInt(matcher.group(3));
  // 4. validate date : skipped code
  return new Date(day, month, year);
} else {
 // throw Error
 throw new DateParseException(date +” is not valid”)
}
```

_The code is written in Java._
How does this code work:

1. Date formatted as DD-MM-YYYY can be matched using regex:

```
(\d{2})-(\d{2})-(\d{4})

where \d matches any digit between 0 to 9,
{n} means how many times the last character type is expected
() is called capturing group and enclosed part of string that would be extracted.
Each group is numbered, the first group is assigned “1”, the second one is given number “2”, and so on
```

2. The given date as string is matched against the defined regex.
3. If the match is successful, then Day, month, and year are extracted from the source string using *Group Construct*provided by Regular Expression API. It is a standard construct in any Regular Expression API in any language.
4. The extracted date parts can be validated; related code is skipped for you to exercise.

This is an illustration of Regular Expression based parsing, which is limited to format, which can be defined by regular expressions. It is _a basic example_ of parsing. A programming language or format like XML parsing is complex in nature. You can refer to the book named “Crafting Interpreters” to get an idea about a full-scaled parsing.
[Crafting Interpreters](https://craftinginterpreters.com/)

You can also read about “Lezer : Code Mirror’s Parsing System”
[Lezer](https://marijnhaverbeke.nl/blog/lezer.html)

## Phases of Parsing

_Parsing_ can be scoped as a composition of Scanning and Syntactic Analysis or just Syntactic Analysis.

### Scanning

_Scanning_ is a process of converting a stream of characters into _tokens._
![](https://miro.medium.com/max/1400/1*NMqJryiyOtf3s757A3xEgQ.png)
A token represents a “concept” introduced by format. Logically, a token can be considered as a label assigned to one or more characters. From a processing point of view: A _token_ is an object, can contain _lexeme³_, location information, and more.

In Java language: `if`, `while`, `int` are examples of tokens. In date parsing, tokens are defined by Regex; `\d{2}` (day, month), `—` (separator), `\d{4}` (year) are tokens. Note: Day and month are the same token type. A token is defined by “the pattern of characters” not by locations of characters.

Scanning is also called _Tokenization_ or _Lexical Analysis_. A part of the program which does scanning is called _Lexer or Scanner or Tokenizer._

### Syntactic Analysis

_Syntactic Analysis_ analyses the structure formed as keeping tokens in order as their positions. It also validates and extracts engraved data to create the preferred Data Structure.
![](https://miro.medium.com/max/1400/1*BbkHRIrzKed91Y3VILCEMQ.png)
In date parsing example: “day is followed by month and year.” The order is checked by Regex Engine, also extraction is done based on order and matches.

Errors reported by this phase are called _Syntactic Errors_. Going back to Date parsing example, `99-JAN-2021` is an invalid Date; however, `99–99–9999` is a valid Date because rule `(\d{2})-(\d{2})-(\d{4})` says so. It may seem absurd, but parsers generally validate _Syntactic Correctness._

## Closing Notes

The scale of parsing determines the inclusion or exclusion of Scanning as part of it:

- It is beneficial for big-scale parsing like language (natural or programming) parsing to keep Scanning, and Syntactic Analysis separated. In this case, Syntactic Analysis is called _parsing._
- For small-scale parsing like Date, it may not be beneficial to distinct Lexer and Syntactic Analysis. In this case, it is called _Scannerless Parsing_.
  
Many times, parsing tasks are delegated to parser code generators like [Antlr](https://www.antlr.org/) or [Lex](http://dinosaur.compilertools.net/) . These tools require a set of rules or grammar and generate parser code. 

![](https://miro.medium.com/max/1400/1*xKb5f_JFv8Detbicdy4ggQ.png)

The generated parsers produce a tree upon parsing, which may not be the desired data structure; however, these libraries provide sufficient APIs to convert the constructed tree to the desired data structure.

There is more in Parser world; the type of parsing styles: top-down or bottom-up, Leftmost or Rightmost derivation. These design styles limit parsing capabilities of the parser. You may visit the following link to get a synoptic view of these design choices and their comparison:
[A Guide To Parsing: Algorithms And Terminology](https://tomassetti.me/guide-parsing-algorithms-terminology/#tablesParsingAlgorithms)

## Footnotes

1. _Regular Expression_ is an algebraic expression² representing a group of strings.
2. An algebraic expression utilizes symbols and operators.
3. _A Lexeme_ is a raw string version of Token.
