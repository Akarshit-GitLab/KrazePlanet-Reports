## Title
Reflected XSS - Global Site Search Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
Affected URL: https://cookbook.pwnedbox.site/

The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the lname GET parameter. During testing, the injected payload was reflected in the Last Name input field, while the fname parameter did not demonstrate the same behavior. A mixed-case script tag (<ScRipT>) successfully bypassed the filtering mechanism and executed JavaScript in the browser, displaying an alert popup. This indicates insufficient input handling and missing context-aware output encoding in the vulnerable parameter.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


Open the following crafted URL in a browser:

https://cookbook.pwnedbox.site/?lname=Priyanshxop%27%22%3C%3EScRipT%3Ealert%281%29%3C%2FScRipT%3E

Allow the page to load with the crafted lname parameter.

Observe the JavaScript alert displaying 1.

Click OK to dismiss the alert.

Inspect the page source.


## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1: Enter payload in Fname and Lname
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1723026b-2f05-44cc-8631-ad7d259ca520" />

Image2: Execution of javascript payload
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7816c6bc-6762-4bc8-840c-4400f6095208" />

Image3: view payload at page source
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a79d3e9c-7cb3-43ca-a816-88f0e479f92a" />

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
