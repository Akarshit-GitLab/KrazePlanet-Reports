## Title
Reflected XSS - Script & Img Tag Filter
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://support.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application accepts First Name (fname) and Last Name (lname) values through GET parameters. During testing, HTML event-handler payloads were submitted through both parameters.

The application reflected the supplied values inside the HTML input elements without proper context-aware output encoding. By injecting a crafted <body> element with an onpageshow event handler, JavaScript execution was achieved in the browser. This confirms a Reflected Cross-Site Scripting (XSS) vulnerability affecting both parameters.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->

1.Open the following crafted URL in a browser:

https://support.pwnedbox.site/?fname=%27%22%3C%3E%3Cbody%20onpageshow%3Dalert(1)%3E&lname=%27%22%3C%3E%3Cbody%20onpageshow%3Dalert(1)%3E

2.Allow the page to load with the crafted payload supplied through both parameters.

3.Observe the JavaScript alert displaying 1.

4.Dismiss the alert.

5.Inspect the page source.

6.Locate the First Name and Last Name input elements.

7.Verify that the supplied payload is reflected inside both input elements without proper HTML attribute encoding.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image:1 Enter crafted Payload in Fname and Lname
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d900c961-7d98-4ab2-8e03-fc946c8aa5c9" />

Image:2 Execute Payload the sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/892043bb-1d68-4839-a602-811a201bcab2" />

Image:3 Page source confirm that payload executes sucessfully
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/afcd8672-bc77-45f7-b041-b21629496211" />

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
