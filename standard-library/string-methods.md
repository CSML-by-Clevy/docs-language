# String methods

You can find more info about the particular regex syntax used in the `*_regex` methods on [this link](https://docs.rs/regex/1.3.4/regex/#syntax).

### .to\_uppercase\(\)

Return the same string in all uppercase characters.

```cpp
string.to_uppercase() => String

// example
do val = "Where is Brian?"
do val.to_uppercase() // "WHERE IS BRIAN?"
```

### .to\_lowercase\(\)

Return the same string in all lowercase characters.

```cpp
string.to_lowercase() => String

// example
do val = "Where is Brian?"
do val.to_lowercase() // "where is brian?"
```

### .length\(\)

Return the length of the target string.

```cpp
string.length() => Integer

// example
do val = "Where is Brian?"
do val.length() // 15
```

### .contains\(String\), .contains\_regex\(String\)

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

### .starts\_with\(String\), .starts\_with\_regex\(String\)

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

### .ends\_with\(String\), .ends\_with\_regex\(String\)

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

### .match\(String\), .match\_regex\(String\)

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

In Regex, the `\` character has a special meaning by itself. However, for technical reasons, in all other strings, it must be properly escaped to be used as a special character. For example, if you mean to write the exact string `\n` you must in fact write `\\n`, otherwise `\n` is interpreted a line break.

This documentation explains why it especially matters in Regex syntax to escape backslashes: [https://docs.python.org/2/howto/regex.html\#the-backslash-plague](https://docs.python.org/2/howto/regex.html#the-backslash-plague)

We follow this nomenclature for CSML Regex handling, so a single Regex backslash must be written as a `"\\"` string, and an escaped backslash \(that behaves as a real `"\"` string character\) must in fact be escaped twice, once for being in a string, and once for being in a Regex: you have to write `"\\\\"` to result in the Regex syntax `\\`.

In a future release of CSML we might introduce a "raw string" method to bypass this limitation. In the meantime,
{% endhint %}

### .is\_number\(\), .is\_int\(\), .is\_float\(\)

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

### .split\(String\)

Split a string by a given separator and return an array containing all elements in order. The separator can be a single or multiple characters. If the separator can not be found in the string, the returned array will only contain the original string.

```cpp
string.split(String) => Array[String]

// example
do val = "this is a long string"
do val.split(" ") // ["this", "is", "a", "long", "string"]
do val.split("is") // ["th", " ", " a long string"]
do val.split("camembert") // ["this is a long string"]
```

