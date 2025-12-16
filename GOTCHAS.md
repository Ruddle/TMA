# Gotchas

Addressed in no particular order.

## 1: Lost from JSON: Distinction between undefined and null

Given this type

```ts
interface Data {
  id: number;
  undef1: undefined | number;
  undef2: undefined | number;
  null_data: null;
}
```

This json is valid, and express everything we need:

```json
{
  "id": 5,
  "undef1": undefined,
  "undef2": undefined,
  "null_data": null
}
```

this one is equivalent

```json
{
  "id": 5,
  "null_data": null
}
```

How does that translate in TMA ?

undefined : do not reference the key (like in json exemple 2)

null: reference the key, but with no content

```java
(
    id(5)
    null_data(())
)
```

## 2: Lost from JSON: Distinction between empty string and null

Given this type 

```ts
type Data = null | string
```

JSON empty string is
```json
""
```

JSON null is 
```json
null
```

TMA empty string is an actual empty string
```java
```


TMA null could (by convention) be 
```java
()
```


## 3: Lost from JSON: Distinction between null and empty object

Yes, this is lost without some new convention.

## 4: Lost from JSON: Distinction between Array and Ojbect

_TODO_


## 5: I actually need to know if a value is a string or a number

*Implicitly tagged unions* that use primitive type as tag, are not supported in TMA

Technically, this is possible in JSON: 

```json
[
  {"level": 10},
  {"level": "master"},
  {"level": "beginner"},
  {"level": 0},
  {"level": 32},
  {"level": "0xBEEF"}
]
``` 

And an actual user of this data might switch on the level value type (the tag of the union), and have different behavior for numbers or strings. We cannot recover from losing primitive type here (0xBEEF would be unexpectedly cast to number).

Using *implicitly tagged unions* is **madness**. Hopefully unseen in professionnal code.

At least use explicitly tagged union.

```json
{
  {"type":"level_exact", "level": 10},
  {"type":"level_rank", "level": "master"},
  {"type":"level_rank", "level": "beginner"},
  {"type":"level_exact", "level": 0},
  {"type":"level_exact", "level": 32},
  {"type":"level_rank", "level": "expert"}
}
```

This one translate well to TMA.

## 6: Your arguments are not pendantically true

My opinion is that technicalites can be addressed once we agree on the functionnal aspect first.

I try to make more of a philosophical distinction between JSON and XML, rather than an exact one.