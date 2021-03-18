# Base64, Hex

You can easily encode and decode data to and from Base64 and Hex:

```cpp
say Base64("Winter is coming 🥶").encode() // V2ludGVyIGlzIGNvbWluZyDwn6W2
say Base64("V2ludGVyIGlzIGNvbWluZyDwn6W2").decode() // Winter is coming 🥶

say Hex("Winter is coming 🥶").encode() // 57696e74657220697320636f6d696e6720f09fa5b6
say Hex("57696e74657220697320636f6d696e6720f09fa5b6").decode() // Winter is coming 🥶
```

This can for example be useful when using Basic Auth in HTTP calls:

```cpp
do auth = Base64("user:password").encode()

do HTTP("https://example.com")
  .set({ "Authorization": "Basic {{auth}}" })
  .send()
```

But in general, they are useful methods to encode/decode non-URL-safe or non-ASCII data to ensure exchanges between systems run smoothly.

{% hint style="info" %}
Base64 and Hex are **NOT** encryption methods!
{% endhint %}

