## Title
Reflected XSS - FreshCart: Online Grocery Store
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://freshcart.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the q parameter is reflected in the search results page without proper output encoding. A crafted search query can execute JavaScript in a visitor's browser when the resulting URL is accessed. Successful execution was confirmed using a JavaScript alert.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open the following crafted URL in a browser:

https://freshcart.pwnedbox.site/?q=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2.Allow the page to load with the crafted q parameter.

3.Observe the JavaScript alert displaying 1.

4.Click OK to dismiss the alert.

5.Inspect the page source.

6.Locate the reflected input inside the search status message.

7.Verify that the supplied payload is reflected and JavaScript execution occurs.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1: Upload payload at Search box
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9264e7ce-1e24-48f3-b2f0-2349bc398a42" />

Image 2: Execution of Javascript
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3787d17-4bdc-4c19-929a-74b56386dfcf" />

Image 3: View payload at Page source
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/06fa02a9-04b6-4c6e-b7a0-8866b55572a9" />


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
