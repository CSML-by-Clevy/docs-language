# Hash & HMAC

Encode data using hash and HMAC methods:

```cpp
start:
  say Crypto("Hello World 😆").create_hash("sha256").digest("hex")
  say Crypto("Hello World 😆").create_hmac("md5", "SECRET_KEY").digest("base64")
```

## Methods

### Crypto\(data\)

The `data` parameter is the data to encrypt or encode, and it must be a string.

### create\_hash\(algorithm\)

The algorithm must be one of the following values:

```text
MD5
SHA1
SHA224
SHA256
SHA384
SHA512
SHA3_224
SHA3_256
SHA3_384
SHA3_512
SHAKE_128
SHAKE_256
RIPEMD160
SM3
```

### create\_hmac\(algorithm, key\)

Unlike the `create_hash` method, `create_hmac` requires a key to function. The list of accepted algorithms is the same.

### digest\(encoding\)

The encoding must be either `"base64"` or `"hex"`.

## Error handling

When any operation fails, `Null` is returned and a `say Error(error_message)` is emitted.

