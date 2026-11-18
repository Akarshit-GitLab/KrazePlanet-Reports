## Title
Reflected XSS - Global Site Search Portal
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://search.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the fname and lname GET parameters. User-supplied input is reflected into HTML input fields without proper output encoding, allowing injected JavaScript to execute in the browser.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


1.Open the following crafted URL in a browser:

https://search.pwnedbox.site/?fname=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E&lname=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2.Allow the page to load with the crafted payloads supplied through the fname and lname parameters.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the URL and verify that both fname and lname parameters contain the supplied payload.

6.Inspect the page source and locate the First Name and Last Name input fields.

7.Verify that the injected values are reflected inside both input fields without proper HTML attribute encoding.

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
