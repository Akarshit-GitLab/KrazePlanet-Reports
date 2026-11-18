## Title
Reflected XSS - Script Tag Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://feedback.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the fname and lname GET parameters. User-controlled input is reflected directly into HTML input fields without proper output encoding. During testing, a mixed-case script tag (<ScRipT>) successfully executed JavaScript, demonstrating that the application's filtering mechanism does not adequately handle variations in HTML tag casing. This allows an attacker to inject executable JavaScript through a crafted URL when a victim visits it.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


1.Open the following crafted URL in a browser:

https://feedback.pwnedbox.site/?fname=Priyanshxop&lname=Priyanshxop%27%22%3C%3E%3CScRipT%3Ealert%281%29%3C%2FScRipT%3E

2.Allow the page to load with the crafted fname and lname parameters.

3.Observe the JavaScript alert displaying 1.

4.Click OK to dismiss the alert.

5.Inspect the page source.

6.Locate the reflected fname and lname values.

7.Verify that the supplied payload is reflected inside both input fields without proper HTML encoding.



## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Imgae1: 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6b06297-4491-40fb-86c8-fe20c90c8b7c" />

Image2:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/302668e5-865b-4603-ab55-b37df438b584" />

Image3:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b83da1c7-b6e7-4098-bd2e-b29d0a28bb30" />

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
