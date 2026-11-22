## Title
Reflected XSS - Case-Insensitive Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://directory.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts First Name (fname) and Last Name (lname) through GET parameters. During testing, a mixed-case script tag payload was submitted through both parameters.

The application reflected the supplied values inside the HTML input elements without proper output encoding. The mixed-case <ScRipT> tag was interpreted by the browser, resulting in successful JavaScript execution. This demonstrates a Reflected Cross-Site Scripting (XSS) vulnerability affecting both parameters.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open the following crafted URL in a browser:

https://directory.pwnedbox.site/?fname=Priyanshxop&lname=Priyanshxop%27%22%3C%3E%3CScRipT%3Ealert%281%29%3C%2FScRipT%3E

2.Allow the page to load with the crafted fname and lname parameters.

3.Observe the JavaScript alert displaying 1.

4.Click OK to dismiss the alert.

5.Inspect the page source.

6.Locate the reflected fname and lname values.

7.Verify that the payload is reflected inside both input elements without proper HTML encoding.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image:1 Enter crafted Payload in Fname and Lname 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/733e1e26-2265-4d51-8693-3b9db97564ab" />

Image:2 Execute Payload the sucessfully 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4a856717-1741-4715-89c1-eacf5c64aeb7" />

Image:3 Page source confirm that payload executes sucessfully 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3b72a4f-9f17-4a53-a3ee-c620d1c998bb" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
impact of xss
1.It allows attackers to hijack user session

2.It potentially leads to full account takeover

3.It allows to perform unauthorized actions within the vulnerable application

## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

2.Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute

3.If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page

4.Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
