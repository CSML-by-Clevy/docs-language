# Array methods

### .length()

Return the number of elements contained in the target array.

```cpp
array.length() => Integer

// example
do val = ["Batman", "Robin", "Superman"]
do val.length() // 3
```

### .push(\*)

Add an element at the end of an array.

```cpp
array.push(val) => void

// example
do val = ["Batman", "Superman"]
do val.push("Robin") // ["Batman", "Superman", "Robin"]
```

### .pop()

Remove the last element of an array.

```cpp
array.pop() => void

// example
do val = ["Batman", "Robin", "Superman"]
do val.pop() // ["Batman", "Robin"]
```

### .reverse()

Create a new Array with all elements reversed&#x20;

```c
do vec = [3, 2, 1]

do new_vec = vec.reverse()
say new_vec.to_string() // [1, 2, 3]
```

### .append()

Add the elements of the array to the initial array

```csharp
 do vec = [1, 2, 3]
 do vec2 = [4, 5, 6]

 do append_vec = vec.append(vec2)
 say append_vec.to_string() // [1, 2, 3, 4, 5, 6]
```

### .insert\_at(Integer, \*)

Add an element at position n of an array (shifting the position of all the following elements).

```cpp
array.insert_at(n, val) => void

// example
do val = ["Batman", "Superman"]
do val.insert_at(1, "Robin") // ["Batman", "Robin", "Superman"]
```

### .remove\_at(Integer)

Remove the nth element of an array (unshifting the position of all the following elements).

```cpp
array.remove_at(n) => void

// example
do val = ["Batman", "Robin", "Superman"]
do val.remove_at(1) // ["Batman", "Superman"]
```

### .find(\*)

Returns a new array with all the values found in the original array matching the given value.

```cpp
array.find(x) => Array

// example
do val = ["Batman", "Robin", "Superman", "Batman"]
do val.find("Ironman") // []
do val.find("Robin") // ["Robin"]
do val.find("Batman") // ["Batman", "Batman"]
```

You can find more info about the particular regex syntax used in the `*_regex` methods on [this link](https://docs.rs/regex/1.3.4/regex/#syntax).

### .to\_uppercase()

Return the same string in all uppercase characters.

```cpp
string.to_uppercase() => String

// example
do val = "Where is Brian?"
do val.to_uppercase() // "WHERE IS BRIAN?"
```

### .to\_lowercase()

Return the same string in all lowercase characters.

```cpp
string.to_lowercase() => String

// example
do val = "Where is Brian?"
do val.to_lowercase() // "where is brian?"
```

### .length()

Return the length of the target string.

```cpp
string.length() => Integer

// example
do val = "Where is Brian?"
do val.length() // 15
```

### .contains(String), .contains\_regex(String)

Return whether the string contains another string or expression.

```cpp
haystack.contains(needle) => Boolean
haystack.contains_regex(needle) => Boolean

// example
do val = "Where is Brian?"
// does it contain any "r"?
do val.contains("r") // true
// does it contain the word "where"?
do val.contains("where") // false => no, because it is case sensitive
// does it contain any number?
do val.contains_regex("[0-9]") // true
```

### .starts\_with(String), .starts\_with\_regex(String)

Return whether a string starts with another string or expression.

```cpp
haystack.starts_with(needle) => Boolean
haystack.starts_with_regex(needle) => Boolean

// example
do val = "Where is Brian?"
// does it start with "r"?
do val.starts_with("r") // false
// does it start with any uppercase letter?
do val.starts_with_regex("[A-Z]") // true
```

### .ends\_with(String), .ends\_with\_regex(String)

Return whether a string ends with another string or expression.

```cpp
haystack.ends_with(needle) => Boolean
haystack.ends_with_regex(needle) => Boolean

// example
do val = "Where is Brian?"
// does it end with "r"?
do val.ends_with("r") // false
// does it end with any uppercase letter?
do val.ends_with_regex("[A-Z]") // false
```

### .match(String), .match\_regex(String)

Return all the matches of the string or expression in the target string, or Null if none are found.

```cpp
haystack.match(needle) => Array[String]
haystack.match_regex(needle) => Array[String]

// example
do val = "Where is Brian?"
// does it match with "r"?
do val.match("r") // ["r", "r"] => yes, twice!
// does it match with any uppercase letter?
do val.match_regex("[A-Z]") // ["W", "B"] => yes, and these are the letters!
```

{% hint style="info" %}
**About \_regex methods:**

The `\` (backslash) character has a special meaning. For technical reasons, in all strings, it must be properly escaped, by convention by adding another `\` in front of itself, to avoid being interpreted as a special character. For example, if you mean to write the exact string `"\n"` you must in fact write `\\n`, otherwise `\n` will be interpreted as a line break.

This Python documentation explains why it especially matters in Regex syntax to escape backslashes: [https://docs.python.org/2/howto/regex.html#the-backslash-plague](https://docs.python.org/2/howto/regex.html#the-backslash-plague)

We follow this nomenclature for CSML Regex handling, so a single Regex backslash must be written as a `"\\"` string, and an escaped backslash (that behaves as a literal `"\"` string character) must in fact be escaped twice, once for being in a string, and once for being in a Regex: you have to write `"\\\\"` to result in the Regex syntax `\\`which in turn matches the literal `"\"` string.

In a future release of CSML we might introduce a "raw string" method to bypass this limitation.
{% endhint %}

### .init(size)

Create a new array of size n

```cpp
do arr = [].init(3) // [null, null, null]
```

### .slice(start, end)

Return a new array with all items between `start` and `end`. Some rules apply:

* If `end` is not specified, all the items after `start` are returned.
* When specified, `end` must be ≥ `start`.
* If any of the parameters is < 0, the count is made from the end of the array.

```cpp
do x = ["a", "b", "c", "d", "e"].slice(2, 4)
say "{{x}}" // c, d

do x = ["a", "b", "c", "d", "e"].slice(2)
say "{{x}}" // c, d, e

do x = ["a", "b", "c", "d", "e"].slice(-4)
say "{{x}}" // b, c, d, e

do x = ["a", "b", "c", "d", "e"].slice(-4, 3)
say "{{x}}" // b, c

do x = ["a", "b", "c", "d", "e"].slice(-2, 1)
say "{{x}}" // Error

do x = ["a", "b", "c", "d", "e"].slice(2, 1)
say "{{x}}" // Error

```

### .map(fn), .filter(fn), .reduce(acc, fn)

These methods are useful ways to construct a new arrays from existing arrays. They are inspired from similar methods in other languages and follow the same general syntax.

Here are some examples of what these methods can help you achieve:

```cpp
// create a new array where each original item is multiplied by 2
do newArray = [1, 2, 3, 4].map((item, index) {
  return x * 2
}) // newArray = [2, 4, 6, 8]

// create a new array containing even numbers from the original array
do newArray = [1, 2, 3, 4].filter((item, index) {
  return x % 2 == 0
}) // newArray = [2, 4]

// create a new value by adding all the elements together
do sum = [1, 2, 3, 4].reduce(0, (acc, val, index) {
  do acc = acc + val
  return acc
}) // sum = 1 + 2 + 3 + 4 = 10
```

### .flatten()

Convert an array of arrays to an array containing all elements of the 1st level arrays.

```cpp
do [[1, 2], [3, 4]].flatten() // [1, 2, 3, 4]

// if a 1st-level element is not an array, it will be kept as is
do [[1, 2], "something", [3, 4]].flatten() // [1, 2, "something", 3, 4]
```
