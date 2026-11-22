## Title
Reflected XSS - Category Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://shop.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the sort GET parameter. User-controlled input is reflected into the page's sorting label and category navigation links without proper output encoding.

During testing, a crafted payload containing a mixed-case script tag and a Unicode escape sequence successfully executed JavaScript in the browser. This demonstrates that an attacker can inject executable JavaScript through a specially crafted URL.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


1.Open the following crafted URL in a browser:

https://shop.pwnedbox.site/?category=all&sort=Priyanshxop%27%22%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E

2.Observe the JavaScript alert displaying 1.

3.Dismiss the alert.

4.Inspect the page source.1.Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

5.Verify that the supplied payload is reflected in the sorting label and category filter links.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->

Image1:enter the Payload into  url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/762c5413-2ef8-4ebb-a6cd-bb7ccc9174ed" />

Image2:execute the Payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/708f0544-07f6-44d0-a208-b4d7e4fbdbe1" />

Image3: Page source reveals that payload excutes sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7f8561ed-e37a-404c-94be-a233f13e3154" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
1.It allows attackers to hijack user session

2.It potentially leads to full account takeover

3.It allows to perform unauthorized actions within the vulnerable application

## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

2.Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute

3.If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page

4.Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
