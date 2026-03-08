>JSON web tokens (JWTs) are a standardized format for sending cryptographically signed JSON data between systems. They can theoretically contain any kind of data, but are most commonly used to send information ("claims") about users as part of authentication, session handling, and access control mechanisms.


A JWT consists of 3 parts: a header, a payload, and a signature. These are each separated by a dot, as shown in the following example:
```js
eyJraWQiOiI5MTM2ZGRiMy1jYjBhLTRhMTktYTA3ZS1lYWRmNWE0NGM4YjUiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTY0ODAzNzE2NCwibmFtZSI6IkNhcmxvcyBNb250b3lhIiwic3ViIjoiY2FybG9zIiwicm9sZSI6ImJsb2dfYXV0aG9yIiwiZW1haWwiOiJjYXJsb3NAY2FybG9zLW1vbnRveWEubmV0IiwiaWF0IjoxNTE2MjM5MDIyfQ.SYZBPIBg2CRjXAJ8vCER0LA_ENjII1JakvNQoP-Hw6GG1zfl4JyngsZReIfqRvIAEi5L4HV0q7_9qGhQZvy9ZdxEJbwTxRs_6Lb-fZTDpW6lKYNdMyjw45_alSCZ1fypsMWz_2mTpQzil0lOtps5Ei_z7mM7M8gCwe_AGpI53JxduQOaB5HkT5gVrv9cKu9CsW5MS6ZbqYXpGyOG5ehoxqm8DL5tFYaW3lB50ELxi0KsuTKEbD0t5BCl0aCR2MBJWAbN-xeLwEenaqBiwPVvKixYleeDQiBEIylFdNNIMviKRgXiYuAvMziVPbwSgkZVHeEdF5MQP1Oe2Spac-6IfA
```

# JWT vs JWS vs JWE
| Feature             | JWT   | JWS                       | JWE                       |
| ------------------- | ----- | ------------------------- | ------------------------- |
| Is it a format?     | ✅ Yes | ❌ No (it’s a type of JWT) | ❌ No (it’s a type of JWT) |
| Signed?             | Maybe | ✅ Yes                     | Optional                  |
| Encrypted?          | Maybe | ❌ No                      | ✅ Yes                     |
| Payload readable?   | Maybe | ✅ Yes                     | ❌ No                      |
| Common in web auth? | Yes   | ✅ Very common             | Rare                      |
# Testing
```
Try changing value of a payload of jwt
Try changing value of a header alg : none 

u can brute force weak signing key with hashcat
hashcat -a 0 -m 16500 <YOUR-JWT> /path/to/jwt.secrets.list
```