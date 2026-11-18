## Title
Reflected XSS - Shopify
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://go.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->

The application uses the returnTo GET parameter to determine the destination of the Cancel & return link on the account confirmation page. During parameter discovery using x8scan, an additional GET parameter named returnTo was identified.

Testing revealed that the parameter value was reflected directly into the href attribute of the Cancel & return link without proper HTML attribute encoding. By supplying a crafted payload, it was possible to inject HTML and JavaScript into the page, resulting in successful JavaScript execution in the browser.


## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
Run x8scan against https://go.pwnedbox.site/ to discover GET parameters.

1.Identify the returnTo parameter in the transformed URL.

2.Supply the crafted XSS payload Priyanshxop'"<><ScRipT>confirm("XSS");</ScRipT> through the returnTo parameter.

3.Open the following crafted URL in a browser:

4.https://go.pwnedbox.site/?returnTo=Priyanshxop%27%22%3C%3EScRipT%3Econfirm(%22XSS%22);%3C/ScRipT%3E

5.Allow the Shopify-style account confirmation page to load.

6.Observe the JavaScript confirmation dialog displaying XSS.

7.Dismiss the confirmation dialog.

8.Inspect the page source and locate the Cancel & return link.

9.Verify that the injected payload is reflected inside the link's href attribute.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1: after the X8scan use crafted payload of javascript in in url 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a124d4d9-ebbf-4b18-8449-4a098eae001f" />

Image 2: Execution of javascript payload 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b28f5e2e-eabb-4a54-8bea-07dccb57fee2" />

Image 3: view of payload in the page source

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5b1a0b4-7220-4b79-81fd-5ec50088e698" />

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
