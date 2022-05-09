# HTTP Client

CSML includes a native HTTP client. The following verbs are accepted:

* GET
* POST
* PUT
* PATCH
* DELETE

HTTP will automatically add the following headers to any request:

* Content-Type: application/json;
* Accept: application/json

{% hint style="info" %}
In other words, HTTP requests only allow you to send and query **json-formatted data**.
{% endhint %}

### Performing a HTTP request

To build your request, you can chain explicit methods to the HTTP function. No request is actually performed until `.send()` is added, allowing you to gradually build your request.

Here is a simple example:

```cpp
do pet = HTTP("https://example.com/pets/1").get().send()

// or, if no verb is specified, .get() is implicitly used
do pet2 = HTTP("https://example.com/pets/2").send()

say "This pet is called: {{pet.name}}"
```

You can also create more complex requests:

```cpp
do req = HTTP("https://example.com/pets").set({"authorization":"Bearer XXXXX"})

if (query_mode == "retrieve") {
    // retrieve all the available pets from the API
    do req = req.get()
}
else {
    // this will create a new pet
    do req = req.post({"name": "Oreo", "breed": "poodle"})
}

do res = req.send()

say "{{res}}"
```

The available methods are:

* `.get()` / `.post(body)` / `.put(body)` / `.patch(body)` / `.delete()` : set the request verb, and add an optional JSON-formatted `body`&#x20;
* `.set(data)`: set the request headers, where `{"x-api-key":"somevalue"}` is added as `x-api-key:somevalue` headers
* `.auth(username, password)` set basic auth headers
* `.query(data)`: set the query strings, where `{"key":"value"}` is automatically translated to `?key=value`
* `.disable_ssl_verify()` : for situations where you know that the endpoint comes with an invalid SSL certificate, you can disable SSL verification to bypass this protection. _This option should only be used if you know what you are doing!_

### Analyzing the response

You can check whether the call returned a response by using the `.is_error()` method on the response object.

```cpp
do res = HTTP("https://example.com/does-not-exist").send()

say res.is_error() // true
```

The `.get_info()` method lets you inspect the response of HTTP calls, even when the call returns an error (in which case `.get_info()` will also contain the body):

```cpp
do res = HTTP("https://example.com/does-not-exist").send()

say "{{res.get_info().headers}}" // the response headers
say "{{res.get_info().status}}" // 404
say "{{res.get_info().body}}" // only set in case of error

say "{{res}}" // the body in case of success, or Null in case of error
```

`.is_error()` and `.get_info()` are very useful methods to correctly handle HTTP responses!

```cpp
// You can handle error cases like this:
if (res.is_error()) {
  say "Hmmm... something happened"

  // For example, you can differentiate between 4XX and 5XX errors
  if (res.get_info().status >= 500) say "The server is not happy"
  else if (res.get_info().status >= 400) say "Something is wrong on the client side"
}
```
