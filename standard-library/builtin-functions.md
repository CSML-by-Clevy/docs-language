# Built-in Functions

Macros are built-in functions that perform some common tasks. Below is a list of some common macros that you can use at any stage in your CSML flows.

## OneOf

Return one the elements of the array passed in the first argument at random.

Useful for randomisation of dialogue parts!

```cpp
say OneOf(["I like you", "You're awesome"])
```

## Or

If the first element exists return it, otherwise return the second argument as a default value

```cpp
say Or(var, "default value")
```

## Shuffle

Given an array, return it with its elements in a random order.

Especially useful in questions to randomize the available options!

```cpp
do btn1 = Button("Blue")
do btn2 = Button("Red")
do btns = Shuffle([btn1, btn2])

say Question(
  title = "Select a pill",
  buttons = btns
)
```

## Find

Return whether a string is contained in another string.

```cpp
say Find("needle", in="haystack") // false
say Find("yes", in="well yes, I like cheese") // true
```

## Length

Return the length of a given string or array

```cpp
say Length("My horse is amazing") // 19
say Length([1, 2, 3]) // 3
```

## Random

Return a random floating point number in the range 0-1 (0 included, 1 excluded).

```
say Random() // 0.03196249773128712
say Random() // 0.6416423921015862
```

## Floor

Return the largest integer less than or equal to the given number.

```cpp
say Floor(1.23456) // 1
```

This is useful to generate a random integer in a given range:

```cpp
say Floor(Random() * 5) // a random integer between 0-4 (included)
say Floor(Random() * 8) + 12 // a random integer between 12 - 19 (included)
```

## UUID

Generate a random UUID (v1 or v4, defaults to v4)

```cpp
say UUID() // "aa4b9fb4-4d37-488c-981f-8aebc4eb9eaa"
say UUID("v1") // "d0b40e8e-7ea4-11eb-9439-0242ac130002"
say UUID("v4") // "4b784011-e49b-4913-9d58-7abf4f8a56bc"
```

## Time

The `Time()` helpers lets your manipulate timestamps and dates easily.

```cpp
do myTime = Time() // initialize a Time object at the current UTC time
do myTime = Time().at(2021, 03, 28, 12, 53, 20, 123) // initialize a time object at 2021-03-28T12:53:20.123Z

do myTime = Time().unix() // generate the unix timestamp (in milliseconds)

do myTime = Time().format() // returns an ISO8601 string
do myTime = Time().format("%h%d") // returns a string with a custom format

do myTime = Time().add(60) // adds 60 seconds to the value
do myTime = Time().sub(60) // subtract 60 seconds to the value

do myTime = Time().with_timezone("Europe/Paris") // set the time in the given timezone

do myTime = Time().parse("2021-03-28") // parse a date
do myTime = Time().parse("2021-03-28T12:53:20Z") // parse an ISO-formatted string
do myTime = Time().parse("01/01/2021", "%d/%m/%Y") // parse a custom-formatted string
```

{% hint style="info" %}
CSML's **Time** function is based on Rust's Chrono library. All the formatting options are listed here: [https://docs.rs/chrono/0.4.19/chrono/format/strftime/index.html](https://docs.rs/chrono/0.4.19/chrono/format/strftime/index.html)
{% endhint %}

## Exists

Check if a variable has been saved in the chatbot's memory before for the current user.

```cpp
do Exists("myvar") // false
remember myvar = 123
do Exists("myvar") // true
```

{% hint style="warning" %}
`Exists` only checks for the existance of **top-level variable names** in the chatbot's memory for the current user, i.e `obj` and not `obj.prop`
{% endhint %}
