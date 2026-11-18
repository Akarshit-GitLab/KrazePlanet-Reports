## Title
Reflected XSS - GIGW: Government Website Guidelines Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://guidelines.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
Affected URL: https://guidelines.pwnedbox.site/index.php?s=

The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). User-supplied input through the s parameter is reflected into the HTML response without proper output encoding. By submitting a specially crafted search query, an attacker can execute arbitrary JavaScript in a visitor's browser when the crafted URL is accessed. The vulnerability was confirmed through successful JavaScript alert execution.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open the following crafted URL in a browser:

https://guidelines.pwnedbox.site/index.php?s=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2.Allow the page to load with the crafted value supplied through the s parameter.

3. Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source.

6.Locate the reflected search input in the HTML response.

7.Verify that the injected payload is reflected without proper HTML output encoding.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Uploading payload at the search box
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/90ac4832-3090-493d-94cb-63996d45426b" />
2. JavaScript alert successfully executed.
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bd69ca25-76c9-48c6-a9b6-02ff8d8219b0" />
3. View Payload at Page source
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/96b2c50e-a023-4507-bdd9-0731e220e93b" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
1.It allows attackers to hijack user session

2.It potentially leads to full account takeover

3.It allows to perform unauthorized actions within the vulnerable application
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

2.Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute

3.If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page

4.Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
