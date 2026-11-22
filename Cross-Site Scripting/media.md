## Title
Reflected XSS - Imgur Mobile
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://media.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts a username through the GET parameter and reflects the supplied value in the page content. During testing, a JavaScript payload was submitted through the username parameter and successfully executed in the browser.

Source inspection also revealed that the username is inserted into the profile link's href attribute without proper HTML attribute encoding. This allows an attacker to inject HTML elements and execute JavaScript in the victim's browser.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open the following crafted URL in a browser:

https://media.pwnedbox.site/?username=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C/script%3E

2.Allow the page to load with the crafted value supplied through the username parameter.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source and locate the profile link.

6.Verify that the injected content is reflected inside the link's href attribute without proper HTML attribute encoding.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image : 1 X8scan shows the transformed url

<img width="580" height="172" alt="image" src="https://github.com/user-attachments/assets/198152c8-2543-46a0-af15-8925e1258f18" />

Image : 2 Enter the payload in  Transformed url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d911873-269e-419e-9650-1fc8e133de19" />

Image : 3 Execute the payload
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad1d9ace-34b8-4b1b-ae2e-d6f788ed24e0" />

Image : 4 Page source confirms the execution of payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ccd3c2af-9d00-4c57-9694-b69cf6af14c9" />

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
