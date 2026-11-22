## Title
Reflected XSS - Extended Function Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://account.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts First Name (fname) and Last Name (lname) values through GET parameters. During testing, an obfuscated JavaScript event-handler payload was submitted through both parameters.

Source inspection revealed different output-handling behavior. The fname value was HTML-encoded, whereas the lname value was reflected as raw HTML inside its input element. The payload uses JavaScript string concatenation to construct the alert function name, demonstrating a potential bypass of basic function-name filtering.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open the following crafted URL in a browser:

https://account.pwnedbox.site/?fname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E&lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E

2.Allow the page to load with the crafted fname and lname values.

3.Inspect the page source.

4.Locate the reflected fname and lname parameters.

5.Observe that fname is HTML-encoded, while lname is reflected without proper HTML encoding.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1:- Enter crafted payload in Fname and Lname
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c7ac8c63-b617-442b-a9bc-d5523c6df0e8" />

Image 2:- Execute payload sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2bbecf39-b4be-40c8-a1be-232026c513cc" />

Image 3:- Page source proofs payload execute sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/82715460-b478-4061-adfe-8ab4cc02d359" />

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
