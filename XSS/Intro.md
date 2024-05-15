
## **Types of XSS Attacks**

### - Reflected XSS - Comes from current HTTP request

```
https://insecure-website.com/status?message=All+is+well. <p>Status: All is well.</p>
```

```
`https://insecure-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script> <p>Status: <script>/* Bad stuff here... */</script></p>
```
### - Stored XSS - Script comes from website's database

```
<p>Hello, this is my message!</p>
```

```
`<p><script>/* Bad stuff here... */</script></p>` 
```

### - **DOM-based XSS - Exists in client-side code than server-side code

```
var search = document.getElementById('search').value; var results = document.getElementById('results'); results.innerHTML = 'You searched for: ' + search;
```

```
You searched for: <img src=1 onerror='/* Bad stuff here... */'>
```

### Uses
- Impersonate or masquerade as the victim user.
- Carry out any action that the user is able to perform.
- Read any data that the user is able to access.
- Capture the user's login credentials.
- Perform virtual defacement of the web site.
- Inject trojan functionality into the web site.