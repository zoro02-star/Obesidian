Perfect. Exactly the same style, exactly the same philosophy.

This is the only Pwntools cheatsheet you will ever need. I have removed every single feature you will never actually use.

> 📌 Most important secret: You do not need to be doing binary exploitation to use pwntools. This is the best kept secret in bug bounty. Almost every top hunter uses pwntools every day for web work. It is the single best library ever made for doing anything weird, evil or invalid that requests will not let you do.

It will never lie to you. It will never silently modify your request. It will never hang forever. There is no substitute.


---

### Setup
```bash
pip install pwntools
```
```python
from pwn import *

# ✅ The only setting you ever need to change
context.log_level = 'info'
```

> ❗ **Always set log_level to info.** The default debug output is insane. This is the single most common mistake everyone makes.


---

## 🎯 The 9 Commands You Will Use 95% Of The Time
This is all you actually need to memorize. Nothing else matters.

| Command | What it actually does |
|---|---|
| `r = remote(host, port)` | Open raw TCP connection |
| `r = ssl(host, port)` | Open TLS / HTTPS connection |
| `r.sendline(data)` | Send line, adds newline exactly |
| `r.send(data)` | Send exactly the bytes you give it |
| `r.recvline()` | Receive one full line |
| `r.recvuntil(b"string")` | Receive until you see this exact string |
| `r.recvall()` | Receive everything until connection closes |
| `r.interactive()` | Drop you into an interactive shell |
| `r.close()` | Close connection |

> ✅ There is almost no reason to ever use any other method.


---

## Most Common Operations For Pentesting
Ordered by how often you will actually use them:


---

### 1. Send raw arbitrary HTTP
This is the single most useful trick in all of bug bounty. This is the only way to test HTTP request smuggling, header injection, CLTE, desyncs and WAF bypasses.

```python
r = ssl("target.com", 443, timeout=5)

r.send(b"""GET /api/admin HTTP/1.1
Host: target.com
User-Agent: lol
Connection: close

""")

print(r.recvall().decode())
```

> ❗ There is no other library on earth that will do this. Every other library including requests, curl, httpx will silently fix and modify your malformed request. Pwntools sends exactly the bytes you wrote. It does not help you. It does not judge you. This is the feature.

This alone is reason enough to install pwntools.


---

### 2. Reverse shell listener
Better than netcat. Better than socat. That is it.
```python
r = listen(4444)
r.wait_for_connection()
r.interactive()
```


---

### 3. Bruteforce / Fuzz anything
```python
for word in wordlist:
    r = ssl("target.com", 443)
    r.sendline(f"PASSWORD {word}")
    if b"success" in r.recvline():
        print(f"Found: {word}")
        break
    r.close()
```


---

### 4. Talk to literally anything
It works for FTP, SMTP, Redis, Memcached, SSH, any custom proprietary protocol. You will never need another client library ever again.
```python
# Connect to internal redis
r = remote("127.0.0.1", 6379)
r.sendline(b"INFO")
print(r.recvall())
```


---

### 5. Extremely useful utilities
Pwntools has perfect helper functions that you will start using everywhere. You will never import base64, urllib or binascii ever again.

| Function | What it does |
|---|---|
| `b64e(x)` | base64 encode |
| `b64d(x)` | base64 decode |
| `urldecode(x)` | url decode |
| `urlencode(x)` | url encode |
| `hexlify(x)` | hex encode |
| `unhex(x)` | hex decode |


---

## Very Important Tricks
* Add `timeout=5` to any remote() call and it will never hang forever
* `r.clean()` throws away all pending data in the buffer
* You can always just do `if b"error" in r.recvline()`
* It automatically handles all line ending insanity for you


---

## Common Pitfalls
❌ **Always pass bytes, not strings.** If you get weird errors this is why. Add the `b` prefix.
❌ **Never use the built in http helpers.** They are bad. Just send raw bytes.
❌ **You do not need `process()`** unless you are doing local binary exploitation.
❌ **Never leave log_level on default.** You will hate yourself.

✅ Pwntools trusts you. It will do exactly what you tell it to, and nothing else. No other library does this.


---

## Things you can safely completely ignore
Do not waste even one second learning these:
* All the binary exploitation stuff
* ROP, asm, shellcraft, elf, dynelf
* All the protocol helpers
* Tubes other than remote and ssl
* Logging, hooks, callbacks
* Literally 90% of the entire library

None of them are useful for what you do.


---

> 📌 Final Rule For Bug Bounty:
> Any time you think "I wonder if requests will let me do this", the answer is no. Close requests. Open pwntools. It will work.


---

### Full copy paste starter snippet
```python
from pwn import *
context.log_level = 'info'

r = ssl("target.com", 443, timeout=5)

r.send(b"""GET / HTTP/1.1
Host: target.com
Connection: close

""")

response = r.recvall().decode()
print(response)
```


---

This is everything. You do not need to know anything else about pwntools. Anything not on this cheatsheet is a feature you will never use.

Would you like me to show you an example of any specific thing like an HTTP smuggling tester, raw fuzzer, or generic bruteforcer?