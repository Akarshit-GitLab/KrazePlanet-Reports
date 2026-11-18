## Title
Reflected XSS - PUBG
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://assets.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application uses the p GET parameter to filter and display PUBG community content. During testing, the supplied parameter value was reflected into the data-query HTML attribute without proper output encoding.

The lack of HTML attribute encoding allowed an attacker to inject HTML and JavaScript through a specially crafted URL. The injected payload successfully executed a JavaScript confirmation dialog in the browser, confirming a Reflected Cross-Site Scripting (XSS) vulnerability.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->
1.Open https://assets.pwnedbox.site/.

2.Locate the Filter input field on the Community page.

3.Enter the crafted XSS payload Priyanshxop'"<><ScRipT>confirm("XSS");</ScRipT> into the filter field.

4.Submit the search request.

5.Observe the JavaScript confirmation dialog.

6.Inspect the page source and locate the data-query attribute.

7.Verify that the injected payload is reflected without proper HTML attribute encoding.
## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1: crafted payload upload in search box
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ec408773-102a-4164-aab6-6fa17528efc7" />

Image 2:java script payload execution sucessfull
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6e934377-1af7-4aa8-93ec-78ae609b9c07" />

Image 3: view of payload at page source
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c78add17-4f8b-4825-8634-0786422ba012" />

## Impact
<!-- Explain what an attacker could achieve, who could be affected, and the realistic security consequences. -->

## Remediation
<!-- Suggest specific steps to fix the vulnerability. Include secure coding practices or configuration changes where appropriate. -->
