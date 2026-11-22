## Title
Reflected XSS - Event Handler Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://tickets.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts First Name (fname) and Last Name (lname) values through GET parameters. During testing, an event-handler payload using JavaScript string concatenation was submitted to bypass basic function-name filtering.

Source inspection showed that the fname value was HTML-encoded, whereas the lname value was reflected as raw HTML inside its input element. This allowed the injected HTML event handler to execute JavaScript in the browser, confirming a Reflected Cross-Site Scripting (XSS) vulnerability.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open the following crafted URL in a browser:

https://tickets.pwnedbox.site/?lname=%22%3E%3Cbody%20onpageshow%3D%2F*x*%2Fthis%5B%27a%27%2B%27l%27%2B%27e%27%2B%27r%27%2B%27t%27%5D(1)%2F%2F%3E

2.Allow the Tickets page to load with the crafted payload supplied through the lname parameter.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source.

6.Compare how the fname and lname values are reflected in the HTML response.

7.Verify that the lname value is reflected without proper HTML attribute encoding, while the fname value is encoded.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image: 1 Enter the crafted payload in Fname and Lname
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e2068ab8-596b-4b11-b5a3-acaca82e5179" />

Image: 2 Execute the playload sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/646ff5c8-92bb-4328-91f2-98919b6952a1" />

Image: 3 Page source confirm payload execute sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/26430b3b-aa85-46a3-8263-ceba361d8c04" />

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
