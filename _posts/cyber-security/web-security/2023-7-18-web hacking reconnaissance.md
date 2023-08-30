---
layout: post
title: WEB HACKING RECONNAISSANCE
date: 2023-07-18
categories: [cybersecurity, web-security,Bug_Bounty_Bootcamp_The_Guide_to_Finding_and_Reporting_Web_Vulnerabilities,web-security, reconnaissance, google-dorking, subdomain-enumeration, service-enumeration, directory-brute-forcing, third-party-hosting,]
---

#  WEB HACKING RECONNAISSANCE

## Manually Walking Through the Target

Before diving into any other techniques, it is recommended to first manually walk through the application to gain a better understanding of it. This can be done by:
- Browsing through every page and clicking every link to uncover all features that users can access.
- Trying out functionalities that have not been used before (e.g. creating an event, playing a game, using payment functionality).
- Signing up for an account at different privilege levels to reveal all of the application's features (e.g. creating owners, admins, members, and different channel members in Slack).

By doing this, a rough idea of the attack surface (all points at which an attacker can attempt to exploit the application) can be obtained, as well as the location of data entry points and the interaction between different users. After this, a more in-depth recon process can begin to determine the technology and structure of the application.

# Google Dorking

Google Dorking is a technique used by hackers to find valuable information using Google's search engine. The search engine has its own built-in query language that helps filter searches.

## Useful Google Operators

Here are some of the most useful operators that can be used with any Google search:

### site
- Tells Google to show you results from a certain site only.
  Example: `site:python.org` to search for the syntax of Python's print() function in the official Python documentation.

### inurl
- Searches for pages with a URL that match the search string. 
  Example: `inurl:"/course/jumpto.php" site:example.com` to search for vulnerable pages on a website.

### intitle
- Finds specific strings in a page's title. 
  Example: `intitle:"index of" site:example.com` to search for directory pages on a website.

### link
- Searches for web pages that contain links to a specified URL. 
  Example: `link:"https://en.wikipedia.org/wiki/ReDoS"` to search for discussions about a regular expression denial-of-service (ReDoS) vulnerability.

### filetype
- Searches for pages with a specific file extension. 
  Example: `filetype:log site:example.com` to search for log files on a target site.

### Wildcard (*)
- You can use the wildcard operator (*) within searches to mean any character or series of characters.
  Example: `"how to hack * using Google"` to return any string that starts with "how to hack" and ends with "using Google".

## Quotes (" ")
Adding quotation marks around your search terms forces an exact match.
For example, this query will search for pages that contain the phrase "how to hack": "how to hack". 
And this query will search for pages with the terms "how", "to", and "hack", although not necessarily together: how to hack.

## Or (|)
The or operator is denoted with the pipe character (|) and can be used to search for one search term or the other, or both at the same time. 
The pipe character must be surrounded by spaces. 
For example, this query will search for "how to hack" on either Reddit or Stack Overflow: 
"how to hack" site:(reddit.com | stackoverflow.com). 
And this query will search for web pages that mention either "SQL Injection" or "SQLi": (SQL Injection | SQLi). 
SQLi is an acronym often used to refer to SQL injection attacks.

## Minus (-)
The minus operator (-) excludes certain search results. 
For example, let’s say you’re interested in learning about websites that discuss hacking, but not those that discuss hacking PHP. 
This query will search for pages that contain "how to hack websites" but not "php": "how to hack websites" -php.

## Advanced Search Queries

- Look for a company's subdomains: `site:*.example.com`
- Check for a Kibana dashboard: `site:example.com inurl:app/kibana`
- Find company resources hosted by third party: `site:s3.amazonaws.com COMPANY_NAME`
- Look for sensitive files with extensions:
  - `site:example.com ext:php`
  - `site:example.com ext:log`
- Combine search terms for a more accurate search: `site:example.com ext:txt password`

# WHOIS and Reverse WHOIS
When companies or individuals register a domain name, they need to supply identifying information, such as their mailing address, phone number, and email address, to a domain registrar. Anyone can then query this information by using the whois command, which searches for the registrant and owner information of each known domain. You might be able to find the associated contact information, such as an email, name, address, or phone number:



# WHOIS and Reverse WHOIS
When companies or individuals register a domain name, they need to supply identifying information, such as their mailing address, phone number, and email address, to a domain registrar. Anyone can then query this information by using the whois command, which searches for the registrant and owner information of each known domain. You might be able to find the associated contact information, such as an email, name, address, or phone number:

```
$ whois facebook.com
```

This information is not always available, as some organizations and individuals use a service called domain privacy, in which a third-party service provider replaces the user’s information with that of a forwarding service.

You could then conduct a reverse WHOIS search, searching a database by using an organization name, a phone number, or an email address to find domains registered with it. This way, you can find all the domains that belong to the same owner. Reverse WHOIS is extremely useful for finding obscure or internal domains not otherwise disclosed to the public. Use a public reverse WHOIS tool like ViewDNS.info (https://viewdns.info/reversewhois/) to conduct this search. WHOIS and reverse WHOIS will give you a good set of top-level domains to work with.

## IP Addresses
- Find the IP address of a known domain using the `nslookup` command
  Example: `nslookup facebook.com`
- Perform a reverse IP lookup to find domains hosted on the same server
  Example: Use `ViewDNS.info`
- Run the `whois` command on an IP address to find information about the organization
  Example: `whois 157.240.2.35`
  - Check the NetRange field to see if the target has a dedicated IP range
  - If the organization has a dedicated IP range, any IP found in that range belongs to the organization
## IP Addresses and Autonomous Systems

Another way to find IP addresses in scope is by checking autonomous systems (ASNs), which are networks within the public internet. By checking if two IP addresses share an ASN, you can determine if they belong to the same owner.

To determine if a company owns a dedicated IP range, you can run IP-to-ASN translations and check if many addresses within a range belong to the same ASN. 

## Certificate Parsing

One way of finding hosts is to use the information in Secure Sockets Layer (SSL) certificates. The Subject Alternative Name field in SSL certificates lists additional hostnames using the same certificate. 

You can find certificates for a domain using online databases such as `crt.sh`, `Censys`, and `Cert Spotter`. For example, a certificate search for `facebook.com` on `crt.sh` returns the SSL certificate for Facebook and several other domain names belonging to Facebook.

The `crt.sh` website also offers a JSON output option for easier parsing, by adding `output=json` to the request URL: `https://crt.sh/?q=facebook.com&output=json`.


# Subdomain Enumeration

## Overview
- Subdomains represent a new attack angle on the network.
- Automated tools such as Sublist3r, SubBrute, Amass, and Gobuster are useful for subdomain enumeration.
- A wordlist of likely terms in subdomains is necessary for many subdomain enumeration tools.
- Wordlists can be found online or generated with tools like Commonspeak2. Using multiple wordlists is recommended.
- Gobuster is a tool for brute-forcing subdomains, directories, and files. Its DNS mode is used for subdomain brute-forcing.
- Patterns in subdomains can lead to further discoveries. Altdns is a tool for discovering subdomains with permuted names.
- Knowledge of the company's technology stack can also reveal subdomains.
- Running enumeration tools recursively can find subdomains of subdomains.

# Service Enumeration

In service enumeration, the goal is to find the services hosted on the target machines. There are two main methods for doing this: active scanning and passive scanning.

## Active Scanning
In active scanning, you directly engage with the target machine by sending requests to connect to its ports in order to determine which ones are open. Tools such as Nmap and Masscan can be used for active scanning.

Example:
```
$ nmap scanme.nmap.org
```

## Passive Scanning
In passive scanning, you gather information about the target machine's ports without directly interacting with the server. This method is stealthier and helps avoid detection. Tools such as Shodan, Censys, and Project Sonar can be used for passive scanning. These databases can provide information about the target's IP addresses, certificates, software versions, and more.

Example:
A Shodan search for the IP address 45.33.32.156 (scanme.nmap.org) yields information about the server.
It is recommended to combine information from different databases for a comprehensive result.

# Directory Brute-Forcing

Directory brute-forcing is a method to discover hidden directories on a web server. This can uncover hidden admin panels, configuration files, password files, outdated functionalities, database copies, and source code files. By discovering the structure and technology of an application, you can potentially take over the server. 

Tools such as `Dirsearch` and `Gobuster` use wordlists to construct URLs and request them from a web server. If the server responds with a status code in the 200 range, the directory or file exists and can be browsed.
```
$ dirsearch -u <hostname> -e <file extension>
```
Note: A status code of 404 means the directory or file doesn't exist, while 403 means it exists but is protected. Examine 403 pages carefully to see if you can bypass the protection to access the content.


### An example of a Gobuster command in Dir mode:
```
$ gobuster dir -u target_url -w wordlist
```

To verify that a page is hosted on each location found through brute-forcing, you can use screenshot tools like `EyeWitness` or `Snapper`. These tools take screenshots of each page, allowing you to quickly find hidden services such as developer or admin panels, directory listing pages, analytics pages, and pages that are outdated and poorly maintained, which are common places for vulnerabilities to manifest.


Spidering the Site
------------------

Another way of discovering directories and paths is through web spidering, or
web crawling, a process used to identify all pages on a site.

Third-Party Hosting
-------------------
Take a look at an organization's third-party hosting footprint, such as their S3 buckets. S3 is Amazon's online storage product and can contain hidden endpoints, logs, credentials, user information, source code, and other valuable information. Use Google dorking to find a company's S3 buckets, which typically use the URL format 
```
BUCKET.s3.amazonaws.com 
s3.amazonaws.com/BUCKET
Use search terms like site:s3.amazonaws.com COMPANY_NAME or site:amazonaws.com COMPANY_NAME
```
 If the company uses custom URLs for its S3 buckets, try more flexible search terms like 
 ```
 amazonaws s3 COMPANY_NAME
 amazonaws bucket COMPANY_NAME
 amazonaws COMPANY_NAME
 s3 COMPANY_NAME
 ```

 Another way of finding buckets is to search a company’s public GitHub
repositories for S3 URLs. Try searching these repositories for the term s3. GrayhatWarfare (https://buckets.grayhatwarfare.com/) is an online search
engine you can use to find publicly exposed S3 buckets It allows
you to search for a bucket by using a keyword. Supply keywords related to
your target, such as the application, project, or organization name, to find
relevant buckets.

These are some additional ways to find S3 buckets of a company:

- Use tools like Lazys3 or Bucket Stream to brute-force buckets by using permutations of common bucket names or based on domain names found on the certificates.
- Use the AWS command line tool to access the buckets directly from the terminal by installing it using "pip install awscli" and configuring it to work with AWS. Then, use the "aws s3" command to list or copy files from the buckets.
- Be careful not to delete important company resources during testing and always report any leaked sensitive information.

## GitHub Recon

GitHub Recon is a process of searching an organization's GitHub repositories for sensitive data, accidental commits, and information that could lead to vulnerability discovery. Here are the key steps involved in GitHub Recon:

1. Find Relevant GitHub Usernames: Search for the organization's name or product names on GitHub to locate usernames relevant to the target. Check the GitHub accounts of known employees as well.

2. Audit User Pages: Visit the pages of identified usernames and find repositories related to the projects being tested. Record these repositories and note the usernames of the organization's top contributors for further exploration.

3. Dive into the Code: Explore each repository and pay attention to the Issues and Commits sections. These sections may reveal unresolved bugs, problematic code, recent code changes, and security patches. Look for protection mechanisms and attempt to bypass them. Search the Code section for potentially vulnerable code snippets.

4. Check Issues, Commits, Blame, and History: Analyze the Issues and Commits sections for information leaks and potential vulnerabilities. Use the Blame and History sections to track the development of specific files and identify any hardcoded secrets like API keys, encryption keys, or database passwords.

5. Search for Leaked Credentials: Search the organization's repositories for terms like "key," "secret," and "password" to locate hardcoded user credentials that could provide access to internal systems. Verify the validity of leaked credentials using tools like KeyHacks.

6. Identify Sensitive Functionalities: Look for code related to authentication, password reset, state-changing actions, and private info reads. Pay attention to code dealing with user input, such as HTTP request parameters, headers, paths, database entries, file reads, and uploads, as they can be potential entry points for attackers. Also, search for configuration files and old endpoints for further review.

7. Address Outdated Dependencies: Identify dependencies and imports being used in the code and check if they are outdated. Record any outdated dependencies, as they may be vulnerable to publicly disclosed vulnerabilities that could work on the target.

-  Utilize Recon Tools: Automation tools like Gitrob and TruffleHog can streamline the GitHub recon process. Gitrob locates potentially sensitive files pushed to public repositories, while TruffleHog specializes in finding secrets in repositories using regex searches and high-entropy string scanning.

