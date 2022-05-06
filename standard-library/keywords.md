# Keywords

## Action keywords

### goto

Inside a step, goto some other step. The step's memories are saved and its messages are sent right after this instruction, and before the next step is started.

Special cases:

* If the target step does not exist, the conversation will be closed.
* If the target step is `end`, the conversation is closed.

`goto` behaves as `return` in other languages: anything after a `goto` is ignored, the `goto` is immediately executed.

To reach another flow, you can also use `goto flow otherflow` .

```cpp
onestep:
  goto somestep
  say "after the goto" /* will not be executed */
  goto someotherstep /* will not be executed */

somestep:
  say "hi"

someotherstep: /* will not be executed */
  say "hey"
```

### say

Send a message to the end user. For the full reference on Components

```cpp
say "The quick brown fox jumps over the lazy dog."
```

### debug

Print a simplify version of the following value. Its output is a special debug component (same structure as a `Text` component with a `debug` content\_type).

If the value is a primitive (String, Number...), it will be printed entirely.\
If it is an Object or Array, only the structure of the first level will be printed.

```cpp
do somevalue = 123
debug somevalue // output: 123

do mylargeobj = {
    "val": 1,
    "something": {"toto": "tutu" },
    "other": "hello",
    "onemore": [1, 2, 3]
}
debug mylargeobj // output: {"val": 1, "something": "[Object]", "other": "hello", "onemore": "[Array]"}
```

Unlike the `say` keyword, it can also be used inside native CSML functions:

```cpp
fn is_triple_even(num):
  do triple = num * 3
  debug triple
  return (triple % 2)
```

### hold

Wait for user input: simply `hold` the conversation in place until the user responds.

```cpp
somestep:
  say "What's up doc?"
  hold

  remember updoc = event
  goto end
```

### hold\_secure

_introduced in CSML v1.10.0_

Same as `hold`, except any user input that comes after that will not be saved in any CSML memory or displayable. However, you can perform operations on the value itself. This is a good use case for secret values that should not be saved in clear text anywhere in a database:

```cpp
start:
  say "Type something super secret, I promise I won't tell!"
  hold_secure // indicates that the next input will be secure

  // Value can not be displayed back or remembered in any way
  say "this is a secret: {{event}}" // prevented by CSML
  remember impossible = event // prevented by CSML
  do impossible = event // prevented by CSML

  
  // You can however perform operations on the event,
  // for example checking if the value is accepted
  if (event == "password") say "This is hardly a good secret!"
  else say "OK, good good"
```

{% hint style="warning" %}
This keyword should be used with extreme care. It is for example a very bad practice to ask users for their password in a chat, and it should not be used for this purpose. However, this keyword can be extremely useful to share data (personal information, short-lived secrets...) that requires extra precautions or should not be stored in clear text (or at all).
{% endhint %}

### remember

Save a value to the bot's memory with the given key. It can later be retrieved (as soon as the next step) with `"{{memory_item}}"`.

By default, the scope is bot/user/channel. The same user on a different channel, or a different user on the same channel, or the same user with a different bot will get a fresh memory instance.

```cpp
// remember a hardcoded value
remember truc = "tutu"

// remember a local variable
do myvar = 123
remember tata = myvar

// overriding a local variable's scope
do myvar = 123 // `myvar` has a local scope
remember myvar = myvar // `myvar` is now a globally-available memory
do myvar = myvar // `myvar` will still be available globally
```

### do

Execute the following expression.

Usually used for executing functions without caring for its return value, or for updating values of objects or arrays.

When used in an assignment (as in `x = y`), the value `x` is saved as a local (temporary) variable.

```cpp
// execute a function
do Fn("someFunc")

// assign new values
do array[3] = "X"
do obj.val.toto = 123

// however, this will fail:
remember myarr = [1, 2]
do myarr[42] = 1 // the array needs to have at least the requested number of items

remember myobj = Object("key"="value")
do myobj.missing.otherkey = 1 // all the parent properties must exist and be objects as well
```

### forget

The `forget` keyword lets you forget memories selectively, or globally.

```cpp
forget something // forget a single memory
forget [something, otherthing, thirdthing] // forget several memories at once
forget * // forget EVERYTHING (dangerous!)
```

### use..as (deprecated)

See `as` keyword.

This keyword will be deprecated in a future release.

```cpp
use 42 as answer

say "The answer is {{answer}}"
```

### foreach

Iterate over each element of an array.

```cpp
do array = ["a", "b", "c"]
foreach (val, index) in array {
  say "at position {{index}} is element with value {{val}}"
}
```

### while

A simple loop with a condition check at every turn

```cpp
do i = 0
while (i < 3) {
  say "i = {{i}}"
  do i = i + 1
}
```

### break, continue

Exit from loops early or skip an iteration.

```cpp
remember lightsabers = [
  {"color": "red", "owner": "Kylo Ren"},
  {"color": "purple", "owner": "Mace Windu"},
  {"color": "yellow", "owner": "Rey Skywalker"},
  {"color": "green", "owner": "Yoda"},
  {"color": "red", "owner": "Darth Vader"},
  {"color": "green", "owner": "Luke Skywalker"},
 ]

foreach (ls) in lightsabers {
  // we want to skip any red lightsaber
  if (ls.color == "red") continue
  say "{{ls.owner}} had a {{ls.color}} lightsaber"
  // we want to stop after we find the first green lightsaber
  if (ls.color == "green") break
}

say "There might be even more lightsabers!"
```

## Other keywords

### as

Save any value as a local variable, only available within the step. Local variables remain in memory after the step is done.

```cpp
do Button("A") as btn1
// is equivalent to
do btn2 = Button("B")

// say and save a component at the same time
say Question(
  title = "Pick one",
  buttons = [btn1, btn2] as btnlist
) as myquestion

// repeat the same question
say myquestion
```

### if / else if / else

Simple logic operators `if`, `else if` and `else`. See examples.

```cpp
// regular notation
if (1 > 2) {
  say "I have my doubts"
} else if ("mi casa" == "tu casa") {
  say "Welcome home"
} else {
  say "The force is strong with you"
}

// shorthand notation
if (sky == "blue") goto beach
else goto restaurant
```

### match (deprecated)

{% hint style="warning" %}
This syntax is obsolete, but for backwards-compatibility reasons remains valid CSML. However, prefer using the `event.match(...)` alternative which is much more versatile.
{% endhint %}

Whether or not a variable "equals", in any way, another variable.

```cpp
do Button(
  title = "I agree",
  accept = ["OK", "yes", "right"]
) as btn

/* a direct click on the button will match the button */
/* typing "yes" will match the button */
/* typing "of course" will not match the button */
if (event match btn) { // Note: this is equivalent to event.match(btn)
  say "good"
} else {
  say "not good"
}
```

### Mathematical Operators

In CSML, you can use the 4 basic mathematical operators `+`, `-`, `*` and `/`, as well as the modulo operator `%`. The regular order of operations applies.

Additionally, since CSML v1.8.0, you can use the shortcut `+=` and `-=` operators to add/subtract and assign values at the same time:

```cpp
do i = 8
do i += 5
say "i = {{i}}" // i = 13
```

### String Concatenation

Starting with CSML v1.8.0, you can concatenate two or more strings together by using the `+` sign:

```cpp
do val1 = "John"
do val2 = "Nutella"
say val1 + " likes " + val2 // John likes Nutella
```

It is also possible to use string templating to insert any value in another string:

```cpp
do val1 = "John"
do val2 = "Nutella"
say "{{val1}} likes {{val2}}" // John likes Nutella
```
