## Title
Reflected XSS - Function Name Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://profile.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts First Name (fname) and Last Name (lname) values through GET parameters. During testing, a crafted HTML event-handler payload was submitted through the form. The page source showed that the fname value was HTML-encoded, whereas the lname value was reflected without proper encoding.

The payload used JavaScript string concatenation to construct the alert function name. This bypassed the filtering behavior and triggered JavaScript execution through the onpageshow event.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open the following crafted URL in a browser:

https://profile.pwnedbox.site/?lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E

2.Allow the profile page to load with the crafted payload supplied through the lname parameter.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source and compare how the fname and lname values are reflected.

6.Verify that the lname value is reflected without proper HTML attribute encoding, while the fname value is encoded.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
image 1: Use crafted payload at the url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ec8173b-5c66-47ba-87f6-089aa3c932ba" />

Image 2: javascript payload execution sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/92cad4f7-f539-4a6b-b888-ed592008518b" />

Image 3 : view payload at page source
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b88f02ca-69dd-4e59-83e9-dbaa32c74396" />

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
