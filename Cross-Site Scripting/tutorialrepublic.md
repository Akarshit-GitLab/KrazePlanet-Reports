## Title
Reflected XSS - TutorialRepublic: Web Development Reference Search
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tutorialrepublic.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The search functionality is vulnerable to Reflected Cross-Site Scripting (XSS). The application reflects user-supplied input from the q parameter into the search result page without proper output encoding. As a result, an attacker can inject JavaScript that executes in a visitor's browser when the crafted URL is opened.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


1.Open the following crafted URL in a browser:

https://tutorialrepublic.pwnedbox.site/?q=Priyanshxop%27%22%3C%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E

2.Allow the page to load with the crafted payload supplied through the q parameter.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source and locate the search result message.

6.Verify that the injected payload is reflected in the message without proper HTML output encoding.

7.Confirm that the injected JavaScript executes in the browser.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
1. Image 1: Enter the payload in search Box
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0ee1ea52-6677-49e4-9a22-ea683172a05a" />
2. Image 2: Execution of Javascript
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b07c7da-4d00-425a-b208-0a958dfc0583" />
3. Image 3: Reflected payload at Page source
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dafbc24a-edff-4828-96ae-0fe1554d6ea3" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->
1. It allows attackers to hijack user session
2. It potentially leads to full account takeover
3. It allows to perform unauthorized actions within the vulnerable application
## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
1. Filter out HTML tags like: <script>, <img>, <svg> from the Report Name field before saving anything to the database
2. Filter out JavaScript methods like: alert(), confirm(), prompt() so even if a tag slips through the method won't execute
3. If you're using PHP then use htmlspecialchars() function before rendering any user input back to the page
4. Use Cloudflare as they have so many WAF rules that almost all XSS payloads will be blocked automatically before even reaching the server
