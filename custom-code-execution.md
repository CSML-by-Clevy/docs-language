# External Code Execution

The CSML engine is able to automatically handle the execution of any payload, any code, in any language, thanks to the built-in `App()` macro. With CSML Apps, you can integrate your own business logic, on your own servers, in your language of choice.

When used together with custom _serverless_ function runtimes \(cloud-based such as [AWS Lambda](https://aws.amazon.com/fr/lambda/features/), [Azure Functions](https://azure.microsoft.com/fr-fr/services/functions/), [Google Cloud Functions](https://cloud.google.com/functions/docs/)\), or on-premise with [FnProject](https://fnproject.io/), [OpenFaas](https://docs.openfaas.com/) or [OpenWhisk](https://openwhisk.apache.org/)\), CSML Functions are a good way to execute custom business logic outside of the context of the conversation.

```cpp
findweather:
  say "Let me query the weather in {{location}} for you..."
  do weather = Fn("weatherchannel", location = location)
  say "It will be {{weather.temperature}}°C tomorrow."
```

{% hint style="warning" %}
**Deprecation notice:** the original `Fn()` notation for calling Apps \(formerly called Functions\) in your CSML code has been replaced by the newer `App()` built-in as of CSML v1.5. Both notations will continue to work until CSML v2 is released, but this documentation will only reference the new `App()` usage from now on.
{% endhint %}

## Running `App` on CSML Studio

When using the [CSML Studio](https://studio.csml.dev/auth/register), the heavy setup of creating an `App` runtime is already done for you, which means that you can easily import functions in java, python, nodejs, go... without any additional setup, simply by uploading your code and calling the function in your CSML script.

CSML Studio also comes with [many ready-to-use integrations](https://www.csml.dev/integrations.html) that are installable in one-click.

## Executing `App` calls

To execute external functions in any programming language when provided a `fn_endpoint`.

For example, when a `App` is called, a HTTP POST request will be performed to the provided `fn_endpoint` with the following payload:

```javascript
{
    "client": {
        "bot_id": "unique-bot-id",
        "user_id": "unique-user-id",
        "channel_id": "unique-channel-id"
    },
    "function_id": "name_of_fn",
    "data": {
        "example": "somevalue",
        "someparam": 123,
        "obj_val": { "hi": "there" }
    }
}
```

The endpoint should return a JSON payload, formatted as follows:

```javascript
{
    "data": data_to_return<JSON>
}
```

If the function fails, or returns an invalid payload, CSML will convert it as a `Null` value.

## Installing a custom nodejs App runtime

The following repository provides an easy way to run your own nodejs runtime: [https://github.com/CSML-by-Clevy/csml-fn-endpoint-node](https://github.com/CSML-by-Clevy/csml-fn-endpoint-node)

Follow the instructions to install and add your own apps!

