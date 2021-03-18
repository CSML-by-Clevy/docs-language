# JWT

The JWT module lets you encode, decode and verify Json Web Tokens easily!

```cpp
start:
  do payload = {
    "user": "name",
    "somekey": {
      "somevalue": 42
    },
    "exp": 1618064023,
    "iss": "CSML STUDIO"
  }

  do jwt = JWT(payload).sign("HS256", "SECRET_KEY")
  say jwt

  do decoded = JWT(jwt).decode("HS256", "SECRET_KEY")
  say "{{decoded}}"

  do claims = {
    "iss": "CSML STUDIO"
  }
  do verified = JWT(jwt).verify(claims, "HS256", "SECRET_KEY")
  say "{{verified}}"
```

## Methods

### sign

**exemple:** `JWT(data).sign(algorithm, secret)`

* `data`: json object to sign and convert to a JWT token
* `algorithm`: see below
* `secret`: a secret string used to sign the JWT

This method returns a properly encoded JWT as a string.

### decode

**exemple:** `JWT(token).decode(algorithm, secret)`

* `token`: the token to decode

**Note:** `decode` does not try to verify that the token is valid. It simply decodes its payload back to the original form.

This method returns the full JWT data, in the form:`{ "payload": { ... your data }, "headers": { "alg": ALG, "typ": "JWT" }}`

### verify

**exemple:** `JWT(token).verify(claims, algorithm, secret)`

* `claims`: set of claims \(in JSON form\) to verify the JWT against

**Note:** only valid claims are verified. See the list of official claims in the JWT specs [here](https://www.iana.org/assignments/jwt/jwt.xhtml#claims).

This method returns the full JWT data, in the form:`{ "payload": { ... your data }, "headers": { "alg": ALG, "typ": "JWT" }}`

## Supported algorithms

JWT supports the following algorithms: `HS256`, `HS384`, `HS512` \(corresponding to HMAC using SHA-256, SHA-384 and SHA-512 respectively\).

The secret key must be a valid, url-safe string.

## Error handling

When a sign, decode or verify operation fails, `Null` is returned and a `say Error(error_message)` is emitted.



