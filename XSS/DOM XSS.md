https://github.com/wisec/domxsswiki/wiki

The following are some of the main sinks that can lead to DOM-XSS vulnerabilities:
- document.write() 
- document.writeln() 
- document.domain 
- element.innerHTML 
- element.outerHTML 
- element.insertAdjacentHTML 
- element.onevent

The following jQuery functions are also sinks that can lead to DOM-XSS vulnerabilities:
- add() 
- after() 
- append() 
- animate() 
- insertAfter() 
- insertBefore() 
- before() 
- html() 
- prepend() 
- replaceAll() 
- replaceWith()
- wrap() 
- wrapInner() 
- wrapAll() 
- has() 
- constructor() 
- init() 
- index() 
- jQuery.parseHTML() 
- $.parseHTML()


Write` sink works with `script` elements, so you can use a simple payload, such as the one below:

```
document.write('... <script>alert(document.domain)</script> ...');
```

```
"><svg onload=alert(1)>
```
```
"></select><img%20src=1%20onerror=alert(1)>
```

## Steps to find document.write vuln

- Find exploitable JS sink such as
	- `location`
	- `location.href`
	- `location.pathname`
	- `location.search`
	- `location.protocol`
	- `location.hostname`
- Try to escape and print text on webpage
- Use a XSS payload

## DOM XSS in jQuery

If a JavaScript library such as jQuery is being used, look out for sinks that can alter DOM elements on the page. For instance, jQuery's `attr()` function can change the attributes of DOM elements. If data is read from a user-controlled source like the URL, then passed to the `attr()` function, then it may be possible to manipulate the value sent to cause XSS. For example, here we have some JavaScript that changes an anchor element's `href` attribute using data from the URL:

`$(function() { $('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl')); });`

You can exploit this by modifying the URL so that the `location.search` source contains a malicious JavaScript URL. After the page's JavaScript applies this malicious URL to the back link's `href`, clicking on the back link will execute it:

`?returnUrl=javascript:alert(document.domain)`

jQuery used to be extremely popular, and a classic DOM XSS vulnerability was caused by websites using this selector in conjunction with the `location.hash` source for animations or auto-scrolling to a particular element on the page. This behavior was often implemented using a vulnerable `hashchange` event handler, similar to the following:


`$(window).on('hashchange', function() { var element = $(location.hash); element[0].scrollIntoView(); });`


As the `hash` is user controllable, an attacker could use this to inject an XSS vector into the `$()` selector sink. More recent versions of jQuery have patched this particular vulnerability by preventing you from injecting HTML into a selector when the input begins with a hash character (`#`). However, you may still find vulnerable code in the wild.

To actually exploit this classic vulnerability, you'll need to find a way to trigger a `hashchange` event without user interaction. One of the simplest ways of doing this is to deliver your exploit via an `iframe`:

`<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=alert(1)>'">`

In this example, the `src` attribute points to the vulnerable page with an empty hash value. When the `iframe` is loaded, an XSS vector is appended to the hash, causing the `hashchange` event to fire.

## Angular JS

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/XSS%20in%20Angular.md

If a framework like AngularJS is used, it may be possible to execute JavaScript without angle brackets or events. When a site uses the `ng-app` attribute on an HTML element, it will be processed by AngularJS. In this case, AngularJS will execute JavaScript inside double curly braces that can occur directly in HTML or inside attributes.

`{{constructor.constructor('alert(1)')()}}`

## Reflected DOM XSS

`eval('var data = "reflected string"');`

Ex:
`eval('var searchResultsObj = ' + this.responseText);`

`\"-alert(1)}//`
