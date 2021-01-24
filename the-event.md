# The Event

The `event` keyword is a special variable in CSML that contains whatever the user did last. It can be generally be used as a simple string, but it is in fact a much more complex object that contains a lot of information about what the user did.

In its essence, `event` contains the [full message payload](sending-receiving-messages/message-payloads.md) as received by the bot. There is a type and a content. The content is an object that contains a text, a payload, a url, other objects... it can be anything, and in general, its content can be derived from the events type. However, using it as `"{{event}}"` in a string \(or simply in `say event` without any curly brace and double quote\) will behave as a string.

## Event methods

`event` contains a number of useful methods that can be used to perform various operations.

### event.match\(\)

In many cases we want to find whether the user's input matches some value to perform conditional logic. For example, in the following example, we want to make sure that the user clicked on the right answer:

```cpp
say Question(
  "What is 2+2?",
  buttons=[Button(4) as ok, Button(5) as notok]
)
hold

if (event.match(ok)) say "That's correct!"
else say "Wrong answer. The right answer was {{ok.title}}"
```

In some cases, you may also want to make sure that the user clicked on a button or typed any accepted response, in which case you can add several parameters to the .match\(\) method:

```cpp
say Question(
  "What is your favorite color",
  buttons=[
    Button("red") as btnred,
    Button("blue") as btnblue,
  ]
)
hold

// this will be true if the user clicked
// on one of the buttons, or typed "red" or "blue"
if (event.match(btnred, btnblue)) remember favcolor = event
else say "{{event}} is not a valid answer"

```

The result of `event.match(...)` is whatever was matched, or `Null`. So the above example, if using the language to its full potential, could also be used like this for even more control over what value is remembered:

```cpp
say Question(
  "What is your favorite color",
  buttons=[
    Button("red", accepts=["Red", "Dark orange"], payload="COLOR_RED") as btnred,
    Button("blue", accepts=["Blue", "sky", "navy"], payload="COLOR_BLUE") as btnblue,
  ]
)
hold

// for a valid choice, the user can either click on the button or 
// manually type any of the accepted options for each button
do matched = event.match(btnred, btnblue)

// matched will contain the matched button object or Null

// in case it contains the button object
// we can prefer to remember its manually set payload
// instead of the button's title
if (matched) remember favcolor = matched.payload

// or continue the same way as before
else say "{{event}} is not a valid answer"
```

### event.match\_array\(\)

Similar to `event.match()`, but takes an array as argument. It also returns the matched value, if any. In many places, it is easier to use as listing all the individual items of the array. The same example as above would now read:

```cpp
say Question(
  "What is your favorite color",
  buttons=[
    Button("red", accepts=["Red", "Dark orange"]),
    Button("blue", accepts=["Blue", "sky", "navy"]),
  ] as buttons
)
hold

// with match_array(), you don't have to list all options
do matched = event.match_array(buttons)

if (matched) remember favcolor = matched.title
else say "{{event}} is not a valid answer"
```

### event.get\_type\(\)

Returns the type of event. For example, if the user typed something, `event.get_type()` will be `"text"`. Or if the user clicked on a button \(which yields a `payload` the type of event is `"payload"`.

```cpp
say "Type something below"
hold
say event.get_type() // "text"
```

## Accessing event content values

To access any value in the content body of the event, simply use the dot notation with its expected property name. If it does not exist, this will return `Null`.

```cpp
// Example event object:
{
  "content_type": "text",
  "content": {
    // a valid text event will always have a content.text property
    "text": "Hi there", 
    // additional properties can be added
    "more_data": "some extra data",
    // extra properties of an event can be of any type.
    "some_number": 42,
    "an_object": { "wow": 1.23456, "impressive": true }
  }
}

say "text: {{event.text}}" // Hi there
say "more_data: {{event.more_data}}" // some extra data
say "doesnotexist: {{event.doesnotexist}}" // Null
```



