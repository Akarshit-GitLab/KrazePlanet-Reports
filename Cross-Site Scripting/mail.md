## Title
Reflected XSS - Encoding Bypass Attempts
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://mail.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the cat GET parameter. During testing, a crafted payload using a mixed-case script tag and Unicode escape sequence successfully bypassed the application's input handling and executed JavaScript in the browser.

The vulnerability occurs because user-controlled input is reflected into the response without sufficient context-aware output encoding. Successful execution was confirmed through a JavaScript alert displaying 1.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open https://mail.pwnedbox.site/?cat=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.

2.Use x8scan to identify the available GET parameters.

3.Identify the cat parameter as the testing target.

4.Inject the crafted XSS payload Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT> into the cat parameter.

5.Load the modified URL in the browser.

6.Observe the JavaScript alert displaying 

7.Inspect the page source to verify the reflected input.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image1:Scan the given url Using X8scan to get Transformed url
<img width="807" height="209" alt="image" src="https://github.com/user-attachments/assets/71906120-418b-458a-9d6d-c6aa76fe67e2" />

Image2:enter the Payload into transformed url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4a128763-f298-44f2-aba4-8d938364df15" />

Image3:execute the Payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f1d3146a-1698-4487-98ee-4d0cbb976566" />

Image4: Page source reveals that payload excutes sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf37f450-35f8-4ea8-ac78-a2cb1d9b2f5f" />

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
