# String methods

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

### .trim()

Returns a new string with both leading and trailing whitespace removed.

```cpp
do text = "   Where is Brian?   "
do new_text = text.trim()
 
say new_text // "Where is Brian?"
```

### .trim\_left()

Returns a new string with leading  whitespace removed.

```cpp
do text = "   Where is Brian?   "
do new_text = text.trim_left()
 
say new_text // "Where is Brian?   "
```

### .trim\_right()

Returns a new string with trailing whitespace removed.

```cpp
do text = "   Where is Brian?   "
do new_text = text.trim_right()
 
say new_text // "   Where is Brian?"
```

### .capitalize()

Return the same string with the first letter in uppercase. The rest of the string remains unchanged.

```cpp
string.capitalize() => String

// example
do val = "my name is John"
do val.capitalize() // "My name is John"
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

### .is\_number(), .is\_int(), .is\_float()

Return whether the given string represents a numerical value, an int, a float.

```cpp
string.is_number() => Boolean

// example
do val = "Where is Brian?"
do val.is_number() // false

do val = "42"
do val.is_number() // true
do val.is_int() // true
do val.is_float() // false
```

### .split(String)

Split a string by a given separator and return an array containing all elements in order. The separator can be a single or multiple characters. If the separator can not be found in the string, the returned array will only contain the original string.

```cpp
string.split(String) => Array[String]

// example
do val = "this is a long string"
do val.split(" ") // ["this", "is", "a", "long", "string"]
do val.split("is") // ["th", " ", " a long string"]
do val.split("camembert") // ["this is a long string"]
```

### .slice(start, end) => String

Cut a string between the `start` and `end` characters. Some rules apply:

* If `end` is not specified, all the characters after `start` are returned.
* When specified, `end` must be ≥ `start`.
* If any of the parameters is < 0, the count is made from the end of the string.

```cpp
say "abcdefghijklmnop".slice(2, 4) // "cd"
say "abcdefghijklmnop".slice(7) // "hijklmnop"
say "abcdefghijklmnop".slice(-4) // "mnop"
say "abcdefghijklmnop".slice(-4, 14) // "mn"

say "abcdefghijklmnop".slice(-4, 3) // Error
say "abcdefghijklmnop".slice(2, 1) // Error
```

### .to\_int(), .to\_float() => Integer, Float

Convert a string representing a number to a value cast as an integer or float:

```cpp
do val = "1.2345".to_int() // 1
do val = "1.2345".to_float() // 1.2345

do val = "not a number".to_int() // error
```
