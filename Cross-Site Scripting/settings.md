## Title
Reflected XSS - Mixed Security Parameters
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://settings.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the academy GET parameter. During testing, multiple GET parameters were identified using x8scan. Further testing of the academy parameter revealed that specially crafted input containing a mixed-case script tag and Unicode escape sequence was reflected in the response and successfully executed JavaScript in the browser.

The vulnerability occurs because user-controlled input is not properly encoded before being included in the HTML response. Successful execution was confirmed through a JavaScript alert displaying 1.
## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open https://settings.pwnedbox.site/?academy=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.

2.Run x8scan to identify the available GET parameters.

3.Identify the academy parameter from the discovered parameter list.

4.Inject the crafted XSS payload Priyanshxop%27"<><ScRipT>a\u006cert(1)</ScRipT> into the academy parameter.

5.Open the modified URL in a browser.

6.Observe the reflected input on the webpage.

7.Confirm that a JavaScript alert displaying 1 appears.

8.Inspect the page source to verify the reflected payload.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image1:Scan the given url Using X8scan to get Transformed url
<img width="810" height="271" alt="image" src="https://github.com/user-attachments/assets/9f9cec35-cf59-42dd-a5ed-ccaddc051688" />

Image2:enter the Payload into transformed url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c613b832-3115-4f34-b93a-41e49448c8e8" />

Image3:execute the Payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ab46ff7c-66b5-4f92-a297-ee671fce1858" />

Image4: Page source reveals that payload excutes sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/98f46a00-b342-420c-b312-9817a33da063" />

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
