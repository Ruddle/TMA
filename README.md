# TMA

Text markup

## Goal

Compromise between XML, JSON, and simplicity

## choices

Text is first class

Its like JSON with no boolean, no numbers, no null, no undefined

Tree structure is first class

Its like XML with no attributes and no closing tag

## Grammar

```bnf
<document> ::= <node>

<node> ::= <identifier>? "(" <content> ")"

<content> ::= (<string> | <node>)*

<identifier> ::= ( [a-z] | [A-Z] | "_" ) ( [a-z] | [A-Z] | [0-9] | "_"  )*

<string> ::=  ( [a-z] | [A-Z] | [0-9] | "_" | "-" | " " | "\n")+
```

TODO, add escaped parenthesis to `<string>`

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

```
menu(
    id(file)
    value(File)
    popup(
        menuitem(
          (value(New) onclick(CreateNewDoc))
          (value(Open) onclick(OpenDoc))
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


```
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


```
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



```
(
(Name(Alice)Age(30)City(New York))
(Name(Bob)Age(25)City(Los Angeles))
(Name(Charlie)Age(35)City(Chicago))
)
```

or, to allow for better scaling (at the cost of some convention)


```
(
headers((Name)(Age)(City))
((Alice)(30)(New York))
((Bob)(25)(Los Angeles))
((Charlie)(35)(Chicago))
)
```
