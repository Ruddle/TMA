# TMA

Text markup

```scala
(
  What(Text markup language)
  How(Compromise between XML, JSON, and simplicity)
  Why(Reject Status Quo)
)
```

## Table of Contents

- [Why not use JSON for JSON stuff, and XML for XML stuff ?](#why-not-use-json-for-json-stuff-and-xml-for-xml-stuff)
  - [JSON pros](#json-pros)
  - [JSON cons](#json-cons)
  - [XML pros](#xml-pros)
  - [XML cons](#xml-cons)
  - [What is common](#what-is-common)
  - [Discussion](#discussion)
  - [Lemma 1 : Metadata is just data](#lemma-1--metadata-is-just-data)
  - [Lemma 2 : Text is first class anyway](#lemma-2--text-is-first-class-anyway)
  - [Convergence](#convergence)
    - [Starting from XML](#starting-from-xml)
    - [Starting from JSON](#starting-from-json)
- [What is TMA](#what-is-tma)
- [Grammar](#grammar)
- [Samples](#samples)
  - [json 1](#json-1)
  - [json 2](#json-2)
  - [html](#html)
  - [csv](#csv)

### Why not use JSON for JSON stuff, and XML for XML stuff ?

They both are effective, but imperfect enough to ponder.

I believe they can be unified. Hopefully you will too in 2 minutes.

> You might have gotchas to output whilst reading this document. It is by design. Addressing everyting in this document would make it too long. Please refer to GOTCHAS.md.

Lets sum up pros and cons of each format.

### JSON pros

- Simple
- Pretty enough

### JSON cons

- Lots of quotes, braces, and colons, do we need that much ?
- Not fluent as a markup language (lots of quoting for small string, no multiline for long string)
- No comments

### XML pros

- Fluent as markup language

### XML cons

- Closing tags redundancy
- Calls for a lot of choices (data vs metadata, attribute vs child node)
- Ugly for config files, or as data format
- No types

### What is common

JSON and XML both represent trees.

They are transported as text.

### Discussion

_We want a text fluent JSON variant._

and

_We want a simpler, less verbose XML._

It is possible, and the solution is the same for both goals.

Lets agree on 2 lemmas first.

### Lemma 1 : Metadata is just data

Given this example:

_XML A_

```xml
<element metadata_0="foo">
  <data_1>bar</data_1>
</element>
```

vs

_XML B_

```xml
<element>
  <metadata_0>foo</metadata_0>
  <data_1>bar</data_2>
</element>
```

A receiver of XML A needs to have a representation of possible metadata beforehand.
For instance your browser knows to take attribute `class` to apply css classes to elements.

It is a convention.

Promoting this data to metadata (XML B to XML A) does not actually bring any benefit once you accept that there is a convention anyway.

An other way to say it: You cannot replace convention by promoting data to metadata.

You might as well not have attributes(and metadata) in the first place, and focus on convention. This is where JSON shines.

### Lemma 2 : Text is first class anyway

A serious JSON user will validate incoming data.

There is no trust that any value will be cast right after a JSON.parse.

_JSON A_

```json
{
  "id": 5,
  "name": "foo",
  "job": null
}
```

_JSON B_

```json
{
  "id": "5",
  "name": "foo",
  "job": "null"
}
```

If a receiver knows that id should be a number, it will validate and cast that piece of data.
Whether it is a string or a number in the transport format is irrelevant. No professionnal trusts incoming JSON data.

Having primitive types in JSON is thus useless.

If you remove types from JSON, quotes become half as useful, and text becomes first class.
This is where XML shines.

### Convergence

If we apply those lemmas to JSON, or to XML, we can end up with the same language.

Here we go.

#### Starting from XML

Metadata is data => we remove attributes from the language.

Text is first class => we do not introduce types, and rely on existing conventions.

Lets start with an typical sample

```html
<html>
  <body>
    <div class="test">
      My First Heading
      <p>My first paragraph.</p>
    </div>
  </body>
</html>
```

We demote metadata :

```html
<html>
  <body>
    <div>
      <class>test</class>
      My First Heading
      <p>My first paragraph.</p>
    </div>
  </body>
</html>
```

Now we have the possibility of simplifying the grammar for tags. No need to both open and close those chevrons

```html
html< 
  body< 
    div< 
      class<test class>
      My First Heading 
      p<My first paragraph. p> 
    div> 
  body> 
html>
```

Now lets remove closing tags redundancy, and switch chevrons to parentheses

```java
html(
  body(
    div(
      class(test)
      My First Heading
      p(My first paragraph)
    )
  )
)
```

This is the final form: TMA.

#### Starting from JSON

```json
{
  "id": 5,
  "name": "foo",
  "job": null
}
```

Text is first class, we rely on existing conventions for types, so we only keep strings.

```json
{
  "id": "5",
  "name": "foo",
  "job": "null"
}
```

Quotes look redundant now, lets remove them, and take care of now meaningful whitespaces.

```json
{
  id:5,
  name:foo,
  job:null, // trailing comma to disembiguate whitespaces
}
```

lets switch `:` for `(` and `,`for `)`

```json
{
  id(5)
  name(foo)
  job(null)
}
```

Do we actually need braces ? lets switch them for parenthesis too

```java
(
  id(5)
  name(foo)
  job(null)
)
```

We end up with the same grammar.

This is a PoC progression, I do not suggest to lose distinction between null and "null". See `GOTCHAS.md`

> price to pay :
> we lost distinction between dictionnaries and arrays

## What is TMA

It is a format:

- It accepts that conventions exist

- Text is first class

- Only relies on `(` and `)` to shape data

_TODO: introduce syntax for comments_

## Grammar

pseudocode: 

```bnf
<document> ::= <node>

<node> ::= <identifier>? "(" <content> ")"

<content> ::= (<string> | <node>)*

<identifier> ::= ( [a-z] | [A-Z] | "_" ) ( [a-z] | [A-Z] | [0-9] | "_"  )*

<string> ::=  ( [a-z] | [A-Z] | [0-9] | "_" | "-" | " " | "\n")+
```

TODO: 
- add escaped parenthesis to `<string>`, 
- add rest of UTF8

## Samples

### json 1

```json
{
  "menu": {
    "id": "file",
    "value": "File",
    "popup": {
      "menuitem": [
        { "value": "New", "onclick": "CreateNewDoc" },
        { "value": "Open", "onclick": "OpenDoc" },
        { "value": "Close", "onclick": "CloseDoc" }
      ]
    }
  }
}
```
=>
```java
menu(
    id(file)
    value(File)
    popup(
        menuitem(
          (value(New)   onclick(CreateNewDoc))
          (value(Open)  onclick(OpenDoc))
          (value(Close) onclick(CloseDoc))
        )
    )
)
```

### json 2

```json
{
  "Actors": [
    {
      "name": "Tom Cruise",
      "age": 56,
      "Born At": "Syracuse, NY",
      "Birthdate": "July 3, 1962",
      "photo": "https://jsonformatter.org/img/tom-cruise.jpg"
    },
    {
      "name": "Robert Downey Jr.",
      "age": 53,
      "Born At": "New York City, NY",
      "Birthdate": "April 4, 1965",
      "photo": "https://jsonformatter.org/img/Robert-Downey-Jr.jpg"
    }
  ]
}
```
=>
```java
(
  Actors(
    (
      name(Tom Cruise)
      age(56)
      BornAt(Syracuse, NY)
      Birthdate(July 3, 1962)
      photo(https://jsonformatter.org/img/tom-cruise.jpg)
    )
    (
      name(Robert Downey Jr.)
      age(53)
      BornAt(New York City, NY)
      Birthdate(April 4, 1965)
      photo(https://jsonformatter.org/img/Robert-Downey-Jr.jpg)
    )
  )
)

```

### html

```html
<html>
  <body>
    <div class="test">
      My First Heading
      <p>My first paragraph.</p>
    </div>
  </body>
</html>
```
=>
```java
html(
  body(
    div(class(test)
    My First Heading
    p(My first paragraph.))
  )
)

```

### csv

```csv
Name, Age, City
Alice, 30, New York
Bob, 25, Los Angeles
Charlie, 35, Chicago
```
=>

```java
( Name(Alice)   Age(30) City(New York)    )
( Name(Bob)     Age(25) City(Los Angeles) )
( Name(Charlie) Age(35) City(Chicago)     )
```

or, to allow for better scaling (at the cost of using csv header convention):

```java
( (Name)    (Age) (City)        )
( (Alice)   (30)  (New York)    )
( (Bob)     (25)  (Los Angeles) )
( (Charlie) (35)  (Chicago)     )
```