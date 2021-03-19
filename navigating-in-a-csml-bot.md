# Navigating in a CSML Bot

## Navigating within a flow

A flow is made of at least one step \(called `start`\), but can also contain as many steps as necessary. You should think of steps as individual, simple bits of logic, linked inside the same conversation topic \(which is the flow\).

To go from one step to the next, you can simply use the keyword `goto` \(or `goto step`\) followed by the name of the step.

To finish a flow \(and close the conversation\), add `goto end`.

```cpp
start:
  say "hi"
  goto step otherstep // note: in the following examples, we will use the shorthand notation `goto otherstep`

otherstep:
  say "I'm in otherstep"
  goto end
```

{% hint style="info" %}
**If, at the end of a step, the `goto end` instruction is omitted, the conversation will be closed anyway.** 

In other words,`goto end`can be used to exit of a conversation early at any point, but it is implicitly added at the end of all steps if no other `goto` instruction is present.
{% endhint %}

## Navigating between flows

Similarly to navigating between steps of the same flow, you can go to the beginning of any other flow by using the `goto flow` keyword. This will trigger the `start` step of the target flow and close the current flow, so coming back to the first flow will actually go back to the beginning of that flow.

```cpp
somestep:
  goto flow anotherflow
```

## Advanced navigation

> Introduced in CSML v1.2

If you want to reach a specific step in a specific flow, you can use the @ notation:

```cpp
goto step_name@flow_name
```

This is the universal way of navigating between steps and flow. The above two methods are actually special cases of this notation:

* When the `@flow_name` part is not specified, CSML interprets it as "in the current flow". So `goto stepname` actually is shorthand notation for `goto stepname@current_flow`, where `current_flow` is dynamically matched from the name of the current flow, and works accordingly.
* When the `step_name` part is not specified, CSML interprets it as `start`. So as `@` means "flow", `goto @flow_name` is the same as `goto start@flow_name` which is also the same as `goto flow flow_name`.

### Examples

```cpp
goto stepname // navigate to the step named stepname in the current flow
goto flow flowname // navigate to the start step in flow flowname
goto @flowname // navigate to the start step in flow flowname
goto stepname@flowname // navigate to step stepname in flow flowname
```

## Dynamic step and flow names

> Introduced in CSML v1.5

As you can see above, steps and flows use strings as identifiers, but they are expressed without double quotes `"..."`. In order to navigate to dynamically set steps or flows, CSML adds a special syntax, similar to the _dereference pointer_ concept in other languages.

To go to a variable step or flow, you can reference the variable name prefixed with a `$` sign:

```cpp
do mystep = "somestep"
do myflow = "someflow"

goto $mystep@$myflow
```

Any of the syntaxes presented above are supported. For instance:

```cpp
do buttons = [Button("Burgers"), Button("Vegetables")]
say Question(
  "What is your favorite food?",
  buttons = buttons
)
hold

do matched = event.match_array(buttons)

if (!matched) { /* handle case where input does not match anything */ }

do target = matched.title
goto flow $target

// depending on which button the user selected, this is equivalent to
// goto flow Burgers or goto flow Vegetables
```

{% hint style="warning" %}
Please note that dot notation \(accessing child properties in an object or array, i.e `a.b`\) is not supported in variable flow and step names.
{% endhint %}

