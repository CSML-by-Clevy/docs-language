# SMTP Client

CSML includes a native SMTP client, allowing you to easily send emails from your chatbot.

```cpp
do email = {
 // required
 "from": "John <john.doe@gmail.com>",
 "to": "Jane <jane.smith@hotmail.com>",
 
 // either html or text is required, both can be set
 "html": "<h1>Hello in HTML!</h1>",
 "text": "Hi there in plain text",
 
 // optional
 "reply_to": "Iron Man <tony.stark@gmx.de>",
 "bcc": "James <james.bond@yahoo.com>",
 "cc": "toto@truc.com",
 "subject": "Happy new year",
}

do SMTP(hostname) // mandatory
 .auth(username, password) // optional, defaults to no auth
 .tls(true) // optional, defaults to auto upgrade to TLS if available
 .port(465) // optional, defaults to 465
 .send(email) // mandatory to perform the request

```

