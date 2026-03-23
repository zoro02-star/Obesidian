This is a Beautiful Soup 4 cheatsheet made exclusively for web pentesters and bug bounty hunters.

I have intentionally removed every single feature you will never actually use. This is 100% of what you will ever need 99% of the time.

Beautiful Soup is the only library you should use for this work, because it is the only one that does not care about broken, invalid, malformed HTML. Every other parser will fail on the garbage real websites return. Beautiful Soup just works.


---

### Setup
```bash
pip install beautifulsoup4 lxml
```
```python
from bs4 import BeautifulSoup
import requests

# ✅ The only correct way to initialize
r = requests.get("https://target.com")
soup = BeautifulSoup(r.text, features="lxml")
```

> ❗ **Never use any other parser.** Always use `lxml`. It is 10x faster, and it is the only one that handles broken html correctly. This is the single most common mistake people make.


---

## 🎯 The 8 Commands You Will Use 90% Of The Time
This is all you actually need to memorize. Nothing else matters.

| Command | What it does |
|---|---|
| `soup.find(tag, attrs={})` | Find first matching element |
| `soup.find_all(tag, attrs={})` | Find **all** matching elements |
| `.get('attribute')` | Get value of an attribute |
| `.text` | Get all inner text, stripped of tags |
| `.string` | Get exact raw text inside this tag |
| `.has_attr('name')` | Check if element has an attribute |
| `.parent` | Go up one element |
| `.find_next('input')` | Find the next element after this one |

> ✅ There is almost no reason to ever use any other method.


---

## Most Common Operations For Pentesting
Ordered by how often you will actually use them:

### 1. Extract every link on the page

```python
links = [a.get('href') for a in soup.find_all('a', href=True)]
```
> 💡 This is the single most used bs4 command in bug bounty. You will write this every single day.
> `href=True` automatically skips any <a> tags with no href attribute.


### 2. Extract every single URL on the page
This will find absolutely every url, in any tag, anywhere. It does not miss anything.
```python
all_urls = []
all_urls += [tag.get('src') for tag in soup.find_all(src=True)]
all_urls += [tag.get('href') for tag in soup.find_all(href=True)]
```
> This is better than any regex crawler. This finds urls in scripts, images, iframes, buttons, everything.



### 3. Extract all forms and inputs
This is your bread and butter.
```python
for form in soup.find_all('form'):
    print("Action:", form.get('action'))
    print("Method:", form.get('method', 'get'))

    # Get all inputs inside this form
    for i in form.find_all('input'):
        print(f"  {i.get('name')} = {i.get('value', '')}")
```
> This is how you automatically discover hidden parameters, unauthenticated forms, and test for CSRF. There is no faster way to do this.



### 4. Find elements by attribute
```python
# Find all elements with name=csrf_token
soup.find_all(attrs={"name": "csrf_token"})

# Find all elements with class=internal
soup.find_all(attrs={"class": "internal"})

# Find any element that has an attribute called apikey
soup.find_all(attrs={"apikey": True})
```

> ❗ You do not need to specify the tag. If you leave it out it will search every tag. This is extremely powerful.


### 5. Find hidden elements
```python
# Find all hidden inputs
hidden = soup.find_all('input', type="hidden")

# Find all elements hidden with display:none
soup.find_all(attrs={"style": lambda v: v and "none" in v})
```
This is how you find parameters that the frontend hides from you. This is one of the most useful tricks for bug bounty.




### 6. Partial matching / Wildcard
This is the most powerful underused feature.
```python
# Find any href that contains /admin
soup.find_all('a', href=lambda v: v and '/admin' in v)

# Find any name that starts with token
soup.find_all(attrs={"name": lambda v: v and v.startswith('token')})

# Case insensitive match
soup.find_all(href=lambda v: v and 'secret' in v.lower())
```

> You can pass any function as a matcher. This works for every attribute. This is infinitely more useful than css selectors.




### 7. Extract all javascript
```python
# Get all external js files
js_files = [s.get('src') for s in soup.find_all('script', src=True)]

# Get all inline javascript
for script in soup.find_all('script'):
    if script.string:
        print(script.string)
```




## Very Useful Tricks
*   Get all text on the entire page: `soup.get_text()`
*   Get raw html of an element: `str(element)`
*   Delete an element from the soup: `element.decompose()`




## Common Pitfalls
❌ **Never use `.text` on script tags** use `.string` instead. `.text` will return nothing.
❌ **Never use css selectors** they are slower and they break on invalid html.
❌ **You do not need `select`, `select_one`, siblings, prettify or any of that stuff** You will never need them.

✅ If something exists on the page, `find_all` will find it. It does not matter how broken the html is.




## Things you can safely completely ignore
Do not waste even one second learning these:
* CSS selectors
* Sibling navigation
* modify / edit elements
* prettify
* all other parser options

None of them are useful for what you do.




> 📌 Final Rule For Bug Bounty:
> If you can do something with `find_all` and a lambda, do it that way. It will always work, it will always be faster, and you will never have to look anything up ever again.




### Full copy paste starter snippet
python

import requests
from bs4 import BeautifulSoup

r = requests.get("https://target.com")
soup = BeautifulSoup(r.text, features="lxml")

links = [a.get('href') for a in soup.find_all('a', href=True)]
forms = soup.find_all('form')
hidden = soup.find_all('input', type='hidden')
js = [s.get('src') for s in soup.find_all('script', src=True)]

print(f"Found {len(links)} links")
print(f"Found {len(forms)} forms")
print(f"Found {len(hidden)} hidden parameters")
print(f"Found {len(js)} javascript files")
```



You do not need to know anything else about Beautiful Soup. Anything not on this cheatsheet is a feature you will never use.

Would you like me to show you an example of any specific common script like an automatic form scanner, hidden parameter finder, or endpoint extractor?