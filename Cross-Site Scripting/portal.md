## Title
Reflected XSS - String Concatenation Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://portal.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the cat GET parameter. During testing, x8scan identified multiple GET parameters. Further testing revealed that the cat parameter reflects user-controlled input into the application response without sufficient output encoding.

A crafted payload using a mixed-case script tag and a Unicode escape sequence successfully bypassed the input handling and executed JavaScript in the browser. The execution was confirmed through an alert displaying 1.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open https://portal.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.
2.Run x8scan to identify the available GET parameters.
3.Identify the cat parameter in the scan results.
4.Inject the crafted XSS payload Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT> into the cat parameter.
5.Open the modified URL in a browser.
7.6.Observe the reflected input on the webpage.
8.Confirm that a JavaScript alert displaying 1 appears.
9.Inspect the page source to verify the reflected payload.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image1:Scan the given url Using X8scan to get Transformed url
<img width="808" height="210" alt="image" src="https://github.com/user-attachments/assets/59a229c1-9c84-41d2-afc9-1dd189f2336e" />

Image2:enter the Payload into transformed url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/43487999-49df-4014-beb3-7b76d41f4b85" />

Image3:execute the Payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f613958-a5f5-44fd-8388-1169ed53a3ee" />

Image4: Page source reveals that payload excutes sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/25081440-cd70-44c2-a642-505437880dad" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
.It allows attackers to hijack user session

2.It potentially leads to full account takeover

3.It allows to perform unauthorized actions within the vulnerable application

## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1.Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

2.Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute

3.If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page

4.Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
