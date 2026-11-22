## Title
Reflected XSS - Multi-Parameter Filter Evasion
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://checkout.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts user-controlled input through multiple GET parameters. During testing, the cat parameter was found to reflect supplied input in the HTTP response.

A crafted input containing HTML and JavaScript markup was submitted to evaluate the application's filtering and output handling. The response displayed parts of the supplied input, indicating that user-controlled data is reflected without adequate context-aware handling. JavaScript execution still requires separate confirmation.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


1.Identify the GET parameters of https://checkout.pwnedbox.site/ using x8scan.

2.Select the cat parameter for testing.

3.Open the following crafted URL in a browser:

https://checkout.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3EScRipT%3Ea%5Cu006cert(1)%3C/ScRipT%3E

4.Allow the page to load with the crafted cat parameter.

5.Observe how the supplied value is reflected in the page response.

6.Inspect the page source and verify how the cat parameter is handled.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image : 1 use X8scan to get Transformed url
<img width="812" height="217" alt="image" src="https://github.com/user-attachments/assets/75b95ce7-d780-4791-9139-923516897dbc" />

Image : 2 Enter Payload in transformed url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0a5fc7b2-01d9-4018-a799-39a307f2ef1e" />

Image : 3 Execute the payload 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dd5e037d-18d4-4cd1-b18a-2109bac0f4b5" />

Image 4: page source proof the payload executed sucessfully

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/952546c6-23d7-42b8-a3a9-0d65a5951517" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
It allows attackers to hijack user session

2.It potentially leads to full account takeover

3.It allows to perform unauthorized actions within the vulnerable application

## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

2.Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute

3.If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page

4.Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
