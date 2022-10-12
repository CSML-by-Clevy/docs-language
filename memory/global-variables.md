# Global variables

Inside any given step, a number of global variables are made available to the developer. These variables include past memories, step context, and request metadata. Some of these variables are available globally, including in CSML functions:

* `event`: contains the user input, or `NULL` when empty. See [The Event](../the-event.md).
* `_metadata`: read-only object that is injected into the conversation by the channel.\
  _Usage:_ `_metadata.something`
* `_env`: read-only object containing the bot's defined environment variables.\
  _Usage:_ `_env.something`
* `_memory`: read-only object containing the user's current memory state. This is especially helpful if you need to print the full state of the user's memory for debugging purposes!

## \_metadata: user-level context

The `_metadata` global contains user-level context for each request.

This memory type is injected by the channel at the beginning of the conversation and contains any data that the channel already knows about the user that can be used in the conversation later. **For example, this could include the user's full name or email address, their job title, where they are from**...

```cpp
say "Hello {{_metadata.firstname}} {{_metadata.lastname}}!" // Hello Tony Stark!
```

## \_context: holds flow positioning information

The `_context` can be found under `_metadata` and contains   `current_step, current_flow and default_flow`&#x20;

This memory type is injected and updated automaticaly at the start of each step&#x20;

```
say _metadata._context.current_step

goto @$_metadata._context.default_flow
```

## \_env: bot-level context

The `_env` global contains bot-level context for the entire bot and is shared across all users of the bot.

This memory type is injected by the bot itself and is meant to contain any data that the bot would need for common tasks across all users. **For example, this is where you store the API keys, endpoints, feature flags**...

```cpp
do HTTP("{{_env.COUNTRY_API_ENDPOINT}}/all")
  .set({ "Authorization": "Bearer {{_env.COUNTRY_API_TOKEN}}" })
  .get()
  .send()
```
