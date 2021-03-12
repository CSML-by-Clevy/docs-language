# Value Types

CSML is a dynamically-typed language, even though it is written in Rust, a strong-typed, memory-safe, low-level language. It can handle a lot of different types but some magic happens behind the scenes to make it as easy for the developers to use the language.

In this section, let's learn more about how CSML handles the various quirks a chatbot needs to solve!

```cpp
do number = 3
do string = "something"
do float = -4.7
do object = { "hey": "you" }
do array = [1, 2, number]
do boolean = true
```

## Literals 

CSML is able to natively understand literal types \(`int`, `float`, `string`, ...\).

`integer` and `float` are separate types, but most of the time you should not have to worry about it and consider it as a more generic and virtual `number` type.

You can also express booleans with `true` or  `false` .

Since CSML v1.1, you can also use `\n`, `\t`, `\r` , `\` and `"` characters in strings, with proper escaping \(`\` and `"` must be preceded by a `\` while a single `\` will be ignored\). For example: 

```cpp
say "Hi!\n\nWe are proud to \ present this nice \"CSML v1.1\" update!"
```

## NULL

`NULL` is its own type. Missing values evaluate to `NULL`. The CSML interpreter will automatically parse the object with the usual dot notation `x.y.z` and detect the type of the resulting property. If one of the properties in the chain does not exist or is not an object itself, it will evaluate to `NULL`.

## Objects

You can create an object by returning a JSON-like object from any function, or directly in the CSML code with the `Object()` helper function or by using a shorthand notation similar to JSON format.

```cpp
// Object representation
do obj1 = Object(
  title = "toto",
  body = Object(
    somekey = "somevalue",
    otherkey = 123
  )
)


// Shorthand notation
do obj2 = { 
  "title": "toto",
  "body": { 
    "somekey": "somevalue", 
    "otherkey": 123
  }
}

say obj1.title // "toto"
say obj2.body.otherkey // 123
```

## Arrays

You can also iterate over an array \(represented by items inside square brackets: `["a", "b", "c"]`\) with the `foreach` keyword, and access any of its items by using its index in square brackets notation: `items[2]`.

```cpp
// Arrays
do items = ["a", "b", "c"]

/* iterate over all the elements in the array */
foreach (elem, index) in items {
  say "index: {{index}}, elem: {{elem}}"
}

say items[2] /* "c" */
```

Note that foreach creates a copy of each item as it iterates over the array. So for example, in this next example, the array is not modified:

```cpp
do items = ["a", "b", "c"]

foreach (item) in items {
  do item = item.to_uppercase() 
  say item // "A", "B", "C"
}

say "{{items}}" // ["a", "b", "c"]
```

You can modify the contents of an array either by assigning a new value to any of the items in the array, adding new items at the end with `array.push(elem)` or removing the last element with `array.pop()`.

```cpp
// Array operations
do items = ["a", "b", "c"]

do items = ["a", "b", "c"]
foreach (item, index) in items {
  do items[index] = item.to_uppercase() 
  say item // "a", "b", "c": the copy has not been affected
  say items[index] // "A", "B", "C": but the array content has been modified
}

say "{{items}}" // ["A", "B", "C"]: the array was modified
```

You can also use [Array methods](standard-library/array-methods.md) to apply changes or get data from arrays.

## Printing non-literal values

To print any value as a string, simply use the `Text(value)` component or use the curly-brace template syntax `"{{value}}"`

```cpp
do items = ["a", "b", "c"]

// print a stringified version of the array
say "{{items}}"

// alternatively, you can also use the .to_string() array method
say items.to_string()
```

