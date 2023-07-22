---
layout: post
title: Content Security Policy (CSP)
date: 2023-07-22
categories: [cybersecurity, web-security, XSS]
---

## What is Content Security Policy (CSP)

Content Security Policy (CSP) is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross-Site Scripting (XSS) and data injection attacks.

## How It Works

If you have a website that uses a third-party analytics service, you can use CSP to specify that the browser should only execute scripts loaded from the analytics service’s domain. This way, if an attacker tries to inject malicious scripts into your website via a different domain, the browser will not execute them because they are not from the allowed domain.

`To enable CSP`, a response needs to include an HTTP response header called `Content-Security-Policy` with a value containing the policy. The policy itself consists of one or more directives, separated by semicolons.

Here is an example of an HTTP response header with CSP:

```http
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; img-src *; media-src *; font-src 'self' data:; connect-src *; object-src 'none'; frame-ancestors 'none'
```
This CSP header specifies the following directives:

- `default-src 'self'`: This directive specifies that all resources must come from the same origin as the document itself.
- `script-src 'self' 'unsafe-inline' 'unsafe-eval'`: This directive specifies that scripts can only come from the same origin as the document itself, and also allows inline scripts and eval().
- `style-src 'self' 'unsafe-inline'`: This directive specifies that styles can only come from the same origin as the document itself, and also allows inline styles.
- `img-src *`: This directive specifies that images can come from any source.
- `media-src *`: This directive specifies that media files (audio and video) can come from any source.
- `font-src 'self' data:`: This directive specifies that fonts can only come from the same origin as the document itself, or from data URIs.
- `connect-src *`: This directive specifies that XHR requests can come from any source.
- `object-src 'none'`: This directive specifies that no plugins or applets are allowed.
- `frame-ancestors 'none'`: This directive specifies that the page cannot be embedded in an iframe.

## Mitigating XSS attacks using CSP

To protect against XSS attacks, Content Security Policy (CSP) provides directives to control the loading of scripts from different sources.

1. Allow scripts from the same origin only:
   `script-src 'self'`

2. Allow scripts from a specific domain:
   `script-src https://scripts.normal-website.com`

It's crucial to be cautious when allowing scripts from external domains. If the content served from an external domain can be controlled by an attacker, it could lead to potential attacks. For instance, content delivery networks (CDNs) that use non-customer-specific URLs like ajax.googleapis.com should not be trusted, as attackers might inject malicious content.

https://www.cloudflare.com/learning/cdn/what-is-a-cdn/

CSP also offers two other methods to specify trusted resources:

- Nonces:
  The CSP directive can include a nonce (a random value), and the same value must be used in the script tag that loads a script. If the values don't match, the script won't execute. To be effective, the nonce should be generated securely on each page load and must not be guessable by attackers.

  Here’s an example of how to use a nonce in a script tag:

  ```
    <script src="https://example.com/script.js" nonce="random-value"></script>
  ```

  The value of the nonce attribute should be a `random value that is generated securely on each page load and must not be guessable by attackers`. The same value `must be used in the CSP directive that allows the script to execute:`
  ```
  script-src 'nonce-random-value'
  ```
  This ensures that only scripts with the correct nonce value can execute on the page.

- Hashes:
  The CSP directive can specify a hash of the trusted script's contents. If the actual script's hash doesn't match the value in the directive, the script won't execute. If the script content changes, the hash value in the directive should be updated accordingly.
  Here’s an example of how to use a hash in a CSP directive:
  ```
  script-src 'sha256-hash-value'
  ```
  The hash value should be the SHA-256 hash of the trusted script’s contents. If the actual script’s hash doesn’t match the value in the directive, the script won’t execute. If the script content changes, the hash value in the directive should be updated accordingly.


## `Cross-Site Request Forgery (CSRF) Token Disclosure via CSP img-src Directive`

In Content Security Policy (CSP), the `img-src` directive controls the sources allowed as image sources on a web page. Many CSPs permit image requests from various sources, enabling the use of `<img>` elements to make requests to external servers.

### CSRF Token Disclosure Risk

CSRF is an attack where an attacker tricks a user's browser into making unintended requests to a vulnerable website using the user's credentials and privileges. To protect against CSRF attacks, websites often implement CSRF tokens as security measures.

### Exploiting CSP for CSRF Token Theft

When a website allows image requests via CSP, attackers can create malicious web pages with `<img>` elements pointing to URLs on the vulnerable site containing CSRF tokens. When a user visits this malicious page, their browser automatically makes requests to those URLs, including the CSRF tokens as part of the image requests.

### Consequences of CSRF Token Theft

By stealing CSRF tokens, attackers can launch CSRF attacks against the vulnerable website. This allows them to perform unauthorized actions on behalf of the victim user, leading to potential data theft, account compromise, and other malicious activities.

### Mitigating CSRF Token Disclosure

To prevent this type of attack, web developers should carefully control the `img-src` directive in their CSP. It should be restricted to trusted sources only, minimizing the risk of CSRF token disclosure. Additionally, applications should employ other security measures, such as using CSRF tokens with short lifetimes, implementing secure token storage mechanisms, and validating CSRF tokens on the server-side to prevent leaks or abuse.
