## Title
Reflected XSS - Search Filter Bypass
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://kb.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application is vulnerable to Reflected Cross-Site Scripting (XSS) through the search GET parameter. User-supplied search input is reflected into the results heading and the empty-state message without proper output encoding.

During testing, a crafted payload using a mixed-case script tag and a Unicode escape sequence successfully executed JavaScript in the browser. This confirms that the search parameter can be used to inject executable content into the response.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open https://kb.pwnedbox.site/?search=Priyanshxop%27%22%3C%3E%3CScRipT%3Ea\u006cert(1)%3C/ScRipT%3E.

2.Enter a test value in the search field and submit it.

3.Identify the search parameter in the resulting URL.

4.Replace the search value with the crafted XSS payload akarshit%27"<><ScRipT>a\u006cert(1)</ScRipT>.

5.Open the modified URL in the browser.

6.Observe the JavaScript alert displaying 1.

7.Inspect the page source and verify that the payload is reflected in the results heading and empty-state message.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image1:enter the Payload into  url
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2315bb3e-57bc-46f3-ab77-1bbf6dc56d77" />

Image2:execute the Payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3dc22a9-c294-43ae-aa07-e5cf3a3f65cd" />

Image3: Page source reveals that payload excutes sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0766b90-4842-4f06-89c7-4efb77bf54b8" />
## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
1.It allows attackers to hijack user session

2.It potentially leads to full account takeover

3.It allows to perform unauthorized actions within the vulnerable application


## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
.Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database

2.Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute

3.If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page

4.Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
