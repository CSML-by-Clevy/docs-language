# Macros

Macros are built-in functions that perform some common tasks. Below is a list of some common macros that you can use at any stage in your CSML flows.

## OneOf

Return one the elements of the array passed in the first argument at random.

Useful for randomisation of dialogue parts!

```cpp
say OneOf(["I like you", "You're awesome"])
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

Return a random floating point number in the range 0-1 \(0 included, 1 excluded\).

```text
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

Generate a random UUID \(v1 or v4, defaults to v4\)

```cpp
say UUID() // "aa4b9fb4-4d37-488c-981f-8aebc4eb9eaa"
say UUID("v1") // "d0b40e8e-7ea4-11eb-9439-0242ac130002"
say UUID("v4") // "4b784011-e49b-4913-9d58-7abf4f8a56bc"
```

