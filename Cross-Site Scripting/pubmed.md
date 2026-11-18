## Title
Reflected XSS - PubMed: National Library of Medicine Search Builder
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://pubmed.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-controlled input through the term parameter is reflected into the search results page without proper output encoding, allowing injected JavaScript to execute in the visitor's browser.The issue occurs when specially crafted search input is processed and displayed in the response. Proper output encoding is required to prevent this behavior.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open the following crafted URL in a browser:

https://pubmed.pwnedbox.site/?term=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2.Allow the page to load with the crafted payload supplied through the term parameter.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source and locate the search results warning message.

6.Verify that the injected payload is reflected in the warning message without proper HTML output encoding.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1: Upload payload at the search box
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e9e4b0a-b36c-44da-acdf-070753430589" />

Image2: Execution of Javascript
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5e99f50-160a-479f-a3d2-d5da2390b36e" />

Image 3: View payload at pagesource
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/063fe335-8416-43cf-8dc0-d971382fcdc4" />



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
