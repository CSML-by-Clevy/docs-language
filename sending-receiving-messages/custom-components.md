# Custom Components

{% hint style="warning" %}
Custom components are clearly an advanced feature of CSML. Unless you are missing specific features with the default capabilities of standard CSML message components, you probably will never need to learn more about this topic!

Feel free to skip to the next section 😉
{% endhint %}

## Introduction to CSML Component templates

Under the hood, all message components are created using a JSON-based templating system to describe how the component should behave, what properties it should accept, which are required or optional, their type, etc.

For instance, here is the low-level code to generate the simple `Text()` component:

```javascript
{
  "params": [
    {
      "text": {
        "required": true,
        "type": "String",
      }
    }
  ]
}
```

Which outputs the following JSON payload when used in a CSML flow:

```javascript
{
  "content_type": "text",
  "content": {
    "text": "Hello!"
  }
}
```

Obviously, more complex components are also possible. Here is an example with the standard `Button()` component:

```javascript
{
  "params": [
    {
      "title": {
        "required": true,
        "type": "String"
      }
    },
    {
      "payload": {
        "required": false,
        "type": "String",
        "default_value": [
          {"$_get": "title"}
        ]
      }
    },
    {
      "accepts": {
        "required": false,
        "type": "Array",
        "default_value": [
        ],
        "add_value": [
          {"$_get": "title" },
          {"$_get": "payload" }
        ]
      }
    }
  ]
}
```

You will notice the use of the `$_get` keyword. This function retrieves the value of the given parameter and sets if in place. So for example, in our example, the button's payload will be set to what the developers sets, or by default to the value of the `title` parameter. Same thing goes for the `accepts` property, which results in the following output:

```cpp
// input: Button(title = "hi")

// output:
{
  "content_type": "button",
  "content": {
    "title": "hi",
    // retrieve the title by default
    "payload": "hi",
    // in this case, we are retrieving the value of `title` and the value
    // of `payload` which happens to be set to the value
    // of `title` by default and not overriden by any user value.
    "accepts": ["hi", "hi"]
  }
}
```

## Creating Custom Message Components

In addition to the [standard components](./), you can also add your own custom message components using the same templating format.

```cpp
{
  "MyComponent": {
    "params": [
      {
        "title": {
          "required": true,
          "type": "String",
        }
      },
      {
        "info": {
          "required": false,
          "type": "String",
          "default_value": [
            {"$_set": "this is a default message" },
          ]
        }
      },
      {
        "list": {
          "required": false,
          "type": "Array",
          "add_value": [
              {"$_get": "title" },
          ]
        }
      }
    ]
  }
}
```

### params

Each component is made of a list of params, that all have the same structure:

```cpp
{
  "params": [
    {
      "param_name": {
        "required": Boolean,
        "type": Type,
        "default_value": Array,
        "add_value": Array
      }
    }
  ]
}
```

### **required** 

if set to true, will display an error when the value is not set in the flow

### **type**

describe the type of the parameter to accept

* Null
* Bool
* Number
* String
* Array
* Object

### **default\_value**

default value to set on the object

```cpp
  "default_value": [
      {"$_get": "title"},
  ]
```

### **add\_value**

values to add to the object in all cases \(even if a default value is set\)

```cpp
  "add_value": [
      {"$_set": "text" },
  ]

```

### $\_get, $\_set

* `$_get`: will inject the value of the given parameter
* `$_set`: will set the field to a specific value

## Why Use Custom Components?

In some cases, you may want to specialize your chatbot for one specific channel, which has some complex formats available that you want to make easier to use. For example, CSML Studio has a `QuickReply` custom component, which is a quicker way to create quick replies, which are features of Messenger, Workplace Chat and the Webapp channels.

```cpp
say MessengerQuickReply(
    "here are some quick replies",
    buttons=["btn1", "btn2", "btn3"]
)

// same output, with the standard Question component
say Question(
    "here are some quick replies",
    button_type="quick_reply",
    buttons=[Button("btn1"), Button("btn2"), Button("btn3")]
)
```

If you are making a specialized chatbot for a specific channel, consider using custom components to make your life easier!

## How to use Custom Message Components

There are two ways to import your own custom components in the CSML Engine and use them in your flows.

* as **native components** by saving your all your custom component files in files in a given directory, and set the COMPONENTS\_DIR environment variable in your [CSML Engine](https://github.com/CSML-by-Clevy/csml-engine). In that case, the name of the component will be the name of the file \(without the extension\).
* as **local** **components** by adding a list of custom components to your chatbot's configuration when calling the engine. In that case, you should add a `custom_components` parameter to your bot description with an array of key/value objects where the key is the name of the component and the value its definition.

Native components and local components behave exactly as standard components. The only difference is that local components are used with a prefix  \(`Component.MyComponent`\) in your flows, whereas native components can be called directly \(`MyComponent`\). This allows you to define different components per bot for the same engine instance, while being able to use common components across all your chatbots. Similarly, local components are easier to share with other users of your chatbots as they can be packaged together.

```cpp
// as a native component
start:
  say MyComponent("this is my custom component")
  goto end

// as local component
start:
  say Component.MyComponent("this is my custom component")
  goto end

// JSON output
{
  "content": {
    "title": "this is my custom component",
    "info": "this is a default message",
    "list": [
      "this is my custom component",
    ]
  },
  "content_type": "mycomponent"
}
```

