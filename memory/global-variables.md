# Global variables

Inside any given step, a number of global variables are made available to the developer. These variables include past memories, step context, and request metadata. Some of these variables are available globally, including in CSML functions:

* `event`: contains the user input, or `NULL` when empty. See [The Event](../the-event.md).
* `_metadata`: read-only object that is injected into the conversation by the channel. _Usage:_ `_metadata.something`
* `_env`: read-only object containing the bot's defined environment variables. _Usage:_ `_env.something`

## 

