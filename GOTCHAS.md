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

```c++
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
```c++
```


TMA null could (by convention) be 
```c++
()
```


## 3: Lost from JSON: Distinction between null and empty object

Yes, this is lost without some new convention.

## 4: Your arguments are not pendantically true

My opinion is that technicalites can be addressed once we agree on the functionnal aspect first.

I try to make more of a philosophical distinction between JSON and XML, rather than an exact one.