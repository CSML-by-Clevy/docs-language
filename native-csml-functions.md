# Native CSML Functions

## Native CSML Functions

CSML functions are a simple way to define simple functional code that can be reused across all of your flows. They are defined in any flow file and use the same CSML syntax that is available in any other steps.

Conceptually, they can be viewed as simple placeholders that you can insert at any place in any flow, to perform repetitive tasks.

Declaring a function is similar to declaring a step with some minor differences:

* the name of the function is preceded by the keyword `fn` ,
* there is a list of parameters between parentheses after the name of the function,
* but also, like a step declaration, it ends with a semicolon 

```cpp
fn my_function_name(param1, param2, param3, ...):
    /** do something **/
    return some_val
```

Here is an example of a simple `my_double_add()` function, that takes 2 parameters, multiplies each of them by 2, then adds those numbers together, and returns the result:

```cpp
fn my_double_add(x, y):
  do dbl_x = x * 2
  do dbl_y = x * 2
  return dbl_x + dbl_y

start:
  do value = my_double_add(1, 2)
  say "The result is: {{value}}" // 6
  goto end
```

There is no limit on how many functions a flow can have, and of course you can call a function from within another function. The above example could be rewritten as:

```cpp
fn my_double(num):
  return num * 2

fn my_add(a, b):
  return a + b

fn my_double_add(x, y):
  do dbl_x = my_double(x)
  do dbl_y = my_double(y)
  return my_add(dbl_x, dbl_y)
```

### Limitations

CSML Functions are isolated form the bot and can't interact with the memory or display messages, they can only use \(do, if, foreach, return\) keywords.

```cpp
// Forbidden
fn my_func(a):
    say "hello"
    remember something = 42
    hold
    goto somestep
```

However, other builtins can be used!

```cpp
// Accepted
fn my_func(a):
    do x = 0
    foreach (item) in a {
        do x = x + item.value
        if (x > 42) break
    }
    do HTTP("https://myapi.com/").post().send()
    return x
```

## Reusing functions across flows

Functions are by default attached to the flow where they are written. However, you can import functions from other flows to reuse them elsewhere!

```cpp
// import this function specifically from this flow
import my_function from flow_2

start:
  do value = my_function(1, 2)
  say value
  goto end
```

You can also use a global import by not defining the flow from which a function can be imported.

```cpp
// import a function with this name from any other flow
import my_function
```

{% hint style="warning" %}
**Warning!** If there is more than one flow defining a function with the same name, it will just import one of them randomly.

Don't use the same name on several functions, or specify which flow you want to import from
{% endhint %}

### Advanced usage

You can import multiple functions at once, and even rename them:

```cpp
// import several functions at once
import {my_function, other_function} from flow_2

// you can rename a function
import my_function as better_name from flow_2

start:
  do value = better_name(42)
  say "The result is {{value}}"
```



