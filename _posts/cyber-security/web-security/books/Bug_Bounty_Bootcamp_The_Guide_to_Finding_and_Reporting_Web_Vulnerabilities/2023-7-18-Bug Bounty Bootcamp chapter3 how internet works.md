---
layout: post
title: HOW THE INTERNET WORKS
date: 2023-07-17
categories: [cybersecurity, web-security,Bug_Bounty_Bootcamp_The_Guide_to_Finding_and_Reporting_Web_Vulnerabilities ]
---


# Chapter 3: HOW THE INTERNET WORKS

The Internet consists of two types of devices: 
- Clients: make requests for resources or services
- Servers: provide those resources and services


## Clients and Servers
- The Internet comprises two main types of devices: clients and servers.
- Clients make requests for resources or services, while servers provide those resources and services.
- When visiting a website, the browser acts as a client and requests a web page from a web server.
- The web server responds by sending the browser a collection of resources and files, including HTML, CSS, JavaScript, and embedded media.

## Servers and Web APIs
- In addition to serving web pages, servers also provide access to data through web APIs.
- Web APIs enable applications to interact with and share data and resources.
- Examples include Twitter's APIs, which allow other websites to retrieve public tweets and their authors.
- The book will delve into the security issues surrounding APIs in later chapters.



## The Domain Name System (DNS)
- Every device connected to the Internet has a unique IP address.
- To locate resources on the Internet, humans use domain names instead of IP addresses.
- The Domain Name System (DNS) acts as the Internet's phone book, translating domain names into IP addresses.
- When a user enters a domain name in a browser, a DNS server converts it into the corresponding IP address, allowing the browser to establish a connection.

## Internet Ports
- Once the browser acquires the correct IP address, it connects to the server using a specific port.
- A port is a logical division on a device that identifies a particular network service.
- Ports are identified by their port numbers, ranging from 0 to 65,535.
- Servers use different ports to provide multiple services simultaneously.
- By convention, port 80 is used for HTTP messages, while port 443 is used for HTTPS (the encrypted version of HTTP).


![internet Ports](../../../../../assets/images/internetPorts.png)


For example, a client connecting to port 80 would be understood by the web server as a request for web services. By convention, port 80 is used for HTTP messages and port 443 is used for HTTPS (the encrypted version of HTTP).

## HTTP Requests and Responses
- HTTP (HyperText Transfer Protocol) governs how web clients and servers exchange information.
- The two most common types of HTTP requests are GET and POST.
  - GET requests retrieve data from the server.
  - POST requests submit data to the server.
- Other HTTP methods include OPTIONS, PUT, and DELETE, serving specific purposes.
- HTTP requests consist of a request line, headers, and an optional body.
- The request line specifies the request method, requested URL, and HTTP version.
- Request headers provide additional information about the request, enabling server customization.

An example GET request for the home page of www.google.com:
```http
GET / HTTP/1.1
Host: www.google.com
User-Agent: Mozilla/5.0
Accept: text/html,application/xhtml+xml,application/xml
Accept-Language: en-US
Accept-Encoding: gzip, deflate
Connection: close
```
HTTP requests are composed of a request line, request headers, and an optional request body.
- The request line specifies the request method, the requested URL, and the version of HTTP used. 
- Request headers are used to pass additional information about the request to the server, allowing the server to customize results sent to the client. 


## Structure of HTTP Request
```http
The request line: 
GET / HTTP/1.1 
Host: www.google.com 
User-Agent: <Operating System and Software Version>
Accept: <Accepted Content Types>
Accept-Language: <Accepted Language>
Accept-Encoding: <Accepted Encoding>
Connection: <Connection State>
Cookie: <Cookies Sent from Client to Server>
Referer: <Address of Previous Web Page>
Authorization: <Credentials for User Authentication>
```
---
```
The request line specifies:
- Request Method: GET
- Requested URL: /
- HTTP Version: 1.1

HTTP Request Headers:
- Host: Specifies the hostname of the request.
- User-Agent: Contains the operating system and software version of the requesting software.
- Accept: Tells the server which format the responses should be in.
- Accept-Language: Tells the server which language the response should be in.
- Accept-Encoding: Tells the server which encoding the response should be in.
- Connection: Tells the server whether the network connection should stay open after the server responds.
- Cookie: Used to send cookies from the client to the server.
- Referer: Specifies the address of the previous web page that linked to the current page.
- Authorization: Contains credentials to authenticate a user to a server.
```
---

After the server receives the request, it tries to fulfill it. The server returns resources used to construct a web page using HTTP responses.

---

### HTTP Responses



The HTTP response is returned by the server in response to an HTTP request. 
example of a HTTP Response: 
```http
HTTP/1.1 200 OK
Date: Tue, 08 Feb 2023 12:00:00 GMT
Content-Type: text/html; charset=UTF-8
Server: Apache
Content-Length: 123456

<!doctype html>
<html>
  <head>
    <title>Example Page</title>
  </head>
  <body>
    <h1>Welcome to Example Page</h1>
    <p>This is a sample HTTP response.</p>
  </body>
</html>
```
1. HTTP status code to indicate the success or failure of the request
2. HTTP response headers to provide additional information about the response to the client
3. HTTP response body which contains the actual content of the web page

HTTP status codes
- A status code in the 200 range indicates a successful request.
- A status code in the 300 range indicates a redirect to another page.
- A status code in the 400 range indicates an error on the client's part.
- A status code in the 500 range indicates an error on the server's part.

HTTP response headers
- Date: indicates the time of the response
- Content-Type: indicates the file type of the response body
- Server: indicates the server version
- Content-Length: indicates the size of the response body
- Set-Cookie: sent by the server to set a cookie
- Location: used to redirect the user's browser to a different URL than the one specified in the original request. The value of the header is the URL to which the browser should be redirected.
- Access-Control-Allow-Origin: is used in cross-origin resource sharing (CORS) to specify which domains are allowed to access the resources from a specific origin. The value of this header is either a wildcard * or a list of allowed domains separated by commas.
- Content-Security-Policy: used to control the origin of resources that the browser is allowed to load. The policy defined in the header specifies which sources of resources are trusted by the server and which sources are not. This helps prevent attacks such as cross-site scripting (XSS) by limiting the resources that can be loaded from untrusted sources.
- X-Frame-Options: used to control whether a page can be loaded within an iframe. The value of this header can be either `DENY`, `SAMEORIGIN`, or a specific domain. The DENY value means that the page cannot be loaded within an iframe, while SAMEORIGIN means that the page can only be loaded within an iframe if the parent page has the same origin as the page being loaded. 
HTTP response body
Contains the actual content of the web page, such as HTML and JavaScript code. The browser uses this information to render the web page for the user.

## Content Encoding

Data transferred in HTTP requests and responses isn't always transmitted in plain text form, as websites often encode their messages to prevent data corruption. Encoding ensures reliable transfer of binary data across machines with limited support for different content types. Common encoding methods include Base64 encoding, Base64url encoding, hex encoding, and URL encoding.

### Base64 Encoding
Base64 encoding is commonly used to transport images and encrypted information within web messages. It uses a character set that includes upper and lowercase letters, numbers, and characters "+", "/", and "=" for padding. Here's an example:

```
Original Data: "Hello World"
Base64 encoded data: "SGVsbG8gV29ybGQ="
```


### Base64url Encoding
Base64url encoding is a modified version of Base64, specifically used for the URL format. It uses different non-alphanumeric characters and omits padding. Here's an example:

```
Original Data: "Hello World"
Base64url Encoded Data: "SGVsbG8gV29ybGQ"
```
## Hex Encoding

Hexadecimal encoding, or hex, is a way of representing characters in a base-16 format, where characters range from 0 to F. It is used to transfer binary data reliably across machines that have limited support for different content types. 

Compared to base64 encoding, hex encoding takes up more space and is less efficient. However, it provides for a more human-readable encoded string.

```
The hexadecimal encoding of "Hello World" would be:

48 65 6C 6C 6F 20 57 6F 72 6C 64
```
## URL encoding
 also known as percent-encoding, is a way to encode special characters in a URL. The special characters are converted into a format that can be transmitted over the internet by converting each character into its hexadecimal representation and prefixing it with a percent symbol (%). This allows URLs to be sent over the internet without being misinterpreted by network protocols. URL encoding is important for ensuring the proper transmission of data and for ensuring that URLs are properly formatted and readable.

 # Session Management and HTTP Cookies

Session management is a process that allows a server to handle multiple requests from the same user without asking for login credentials each time. 

A new session is created each time a user logs in to a website, and the server assigns a session ID for the user's browser to serve as proof of identity. The session ID is a long and unpredictable sequence designed to be unguessable. 
![createSessionId](../../../../../assets/images/creates_a_session_id.png)

Most websites use cookies to communicate session information in HTTP requests. HTTP cookies are small pieces of data that the web server sends to the user's browser. When the user logs in, the server creates a session and sends the session ID to the browser as a cookie. The browser then stores the cookie and includes it in every request to the same server, allowing the server to track the user's session. 
![UseSessionId](../../../../../assets/images/use_session_id.png)

When the user logs out, the server invalidates the session cookie so that it cannot be used again. The next time the user logs in, the server will create a new session and associated session cookie. 

# Session and HTTP Cookies in another way

Session and HTTP cookies are mechanisms for maintaining state between client and server in a stateless HTTP protocol.

## Session

When a user visits a website, the server generates a unique session ID and stores it on the server, along with any relevant information about the user's session, such as the items in their shopping cart or their current page in a multi-page form. This session information is then associated with the user's session ID.

The server then sends a cookie to the user's browser containing the session ID. The browser stores this cookie and sends it back to the server with each subsequent request, allowing the server to retrieve the user's session information based on the session ID. This allows the server to maintain a persistent connection with the user and keep track of their activity on the website, even as they navigate to different pages or make requests.

## HTTP Cookies

HTTP cookies are small text files stored on the user's computer by a web browser. They contain information about the user's session, such as their preferences or login credentials, and can be used to persist this information between visits to the website. Cookies are sent back to the server with each request, allowing the server to retrieve and update the information stored in the cookie.


## Token-Based Authentication

In session-based authentication, the server stores information about the user and uses a corresponding session ID to validate the user's identity. On the other hand, in a token-based authentication system, the process works as follows:

1. The client sends a request to log in with their credentials.
2. If the credentials are correct, the server generates a token and sends it back to the client.
3. The client includes this token in all subsequent requests to the server.
4. On the server side, the token is used to identify the user and retrieve information from a database.
5. The server verifies the validity of the token by checking its signature, encrypted using a secret key known only to the server.
6. If the token is valid, the server grants the request and allows the user to access the requested resources.

Instead of storing user information server-side and querying it using a session ID, tokens allow servers to deduce the user's identity by decoding the token. This eliminates the need for the server to store and maintain session information.

To prevent token forgery attacks, some applications encrypt or encode the token to make it more difficult to tamper with. Another way to protect the integrity of a token is by signing it with a secret key and verifying the token signature when it arrives at the server.

# JSON Web Tokens (JWT)

JSON Web Tokens (JWT) is a commonly used authentication token with three components: a header, a payload, and a signature.

## Header

The header identifies the algorithm used to generate the signature. It is a base64url-encoded string that contains the algorithm name.

Example:
```
eyBhbGcgOiBIUzI1NiwgdHlwIDogSldUIH0K
```

This is the base64url-encoded version of:
```
{ "alg" : "HS256", "typ" : "JWT" }

```

## Payload
The payload section contains information about the user's identity. This section is also base64url-encoded before being used in the token.

Example: 
```
eyB1c2VyX25hbWUgOiBhZG1pbiB9Cg
```
This is the base64url-encoded version of:
```
{ "user_name" : "admin", }
```

## Signature
The signature section validates that the user has not tampered with the token. It is calculated by concatenating the header with the payload, then signing it with the algorithm specified in the header and a secret key.

Example: 
```
4Hb/6ibbViPOzq9SJflsNGPWSk6B8F6EqVrkNjpXh7M
```

## Complete Token
The complete token concatenates each section (the header, payload, and signature), separated by a period (.). 

Example: 
```
eyBhbGcgOiBIUzI1NiwgdHlwIDogSldUIH0K.eyB1c2VyX25hbWUgOiBhZG1pbiB9Cg.4Hb/6ibbViPOzq9SJflsNGPWSk6B8F6EqVrkNjpXh7M
```
![jwt](../../../../../assets/images/jwt.png)


## Advantages and Security
When implemented correctly, JSON Web Tokens provide a secure way to identify the user. The server can verify that the token has not been tampered with by checking the signature. Then the server can deduce the user's identity from the information contained in the payload section. The user does not have access to the secret key used to sign the token, so they cannot alter the payload and sign the token themselves. However, if implemented incorrectly, there are ways an attacker can bypass the security mechanism and forge arbitrary tokens.

# Security Issues with JWT

## Tampering with the "alg" field
- Attackers can forge their own tokens by tampering with the "alg" field of the JWT header.
- The "alg" field lists the algorithm used to encode the signature and if the application does not restrict the algorithm type, attackers can specify which algorithm to use.
- The "none" option for the algorithm type could compromise the security of the token if not turned off in a production environment.

## Changing the Type of Algorithm

- The two most common signing algorithms used for JWTs are HMAC and RSA.
- HMAC requires the token to be signed with a key and later verified with the same key.
- RSA requires the token to be created with a private key and verified with the corresponding public key.
- Attackers can change the type of algorithm used to exploit the "alg" field and create valid tokens.

## Misuse of RSA

- When an application uses RSA tokens, the tokens are signed with a private key and verified with a public key.
- If an attacker changes the "alg" field to HMAC, they could sign the forged tokens with the RSA public key, compromising the security of the JWT.

## Brute-Forcing the JWT Key

- Attackers might try to guess or "brute-force" the key used to sign a JWT.
- They have information such as the algorithm used, the payload, and the signature to start with.
- If the key is not complex enough, the attacker may be able to guess it easily.
- Attackers might also try to steal the secret key through other vulnerabilities (e.g. directory traversal, XXE, SSRF).

## Reading Sensitive Information

JWTs often contain user information for access control. If the token is not encrypted, it can be easily decoded and read. This can lead to information leaks if the token contains sensitive information.
JWT signature provides data integrity, not confidentiality.
Proper implementation is crucial for JWT security.

## The Same-Origin Policy

The same-origin policy (SOP) is a rule that restricts how a script from one origin can interact with the resources of a different origin.
In simple terms, a script from page A can only access data from page B if the pages are of the same origin.

Two URLs are considered to have the same origin if they share the same protocol, hostname, and port number.

Examples of pages with the same origin as page A (https://medium.com/@vickieli):
- https://medium.com/

Examples of pages with a different origin than page A:
- http://medium.com/ (different protocol)
- https://twitter.com/@vickieli7 (different hostname)
- https://medium.com:8080/@vickieli (different port number)

The SOP protects us from malicious scripts retrieving sensitive information.
For example, if you're logged into an online bank, the SOP will prevent a malicious script hosted on a different site from reading the information you have access to.

