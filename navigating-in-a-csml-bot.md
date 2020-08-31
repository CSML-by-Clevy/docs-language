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

