## Title
Reflected XSS - Page Heading
<!-- Write a short, clear title describing the vulnerability and where it was found. Example: Stored XSS in User Profile -->

## Description
**Affected URL:** https://docs.pwnedbox.site/
<!-- Explain what the vulnerability is, where it exists, and why it is a security issue. Include the affected URL or endpoint. -->
The application contains a GET-based search/reflection functionality. The visible form displays First Name (fname) and Last Name (lname) fields, but parameter discovery using x8scan revealed an additional GET parameter named q.

The q parameter was not directly visible in the form. After identifying it, a crafted XSS payload was supplied through the parameter. The application reflected the input inside the page heading without proper output encoding, resulting in JavaScript execution in the browser.

## Steps to Reproduce
<!-- Provide clear, numbered steps so another person can reproduce the issue. Include required requests, parameters, and actions. -->


1. Run parameter discovery against https://docs.pwnedbox.site/ using x8scan.

2. Identify the additional GET parameter q.

3.Open the following crafted URL in a browser:

https://docs.pwnedbox.site/?q=Priyanshxop%27%22%3C%3E%3CScRipT%3Ealert(%22XSS%22);%3C/ScRipT%3E

4. Allow the page to load with the crafted q parameter.

5. Observe the JavaScript alert displaying XSS.

6. Click OK to dismiss the alert.

7. Inspect the page source.

8. Verify that the supplied payload is reflected inside the page heading.

## POC (Proof of Concept)
<!-- Add screenshots, HTTP requests/responses, payloads, or videos that demonstrate the vulnerability. -->
Image 1. x8scan identifies the hidden q GET parameter alongside fname and lname.
<img width="875" height="200" alt="image" src="https://github.com/user-attachments/assets/7818cd04-4631-42f0-9fe1-45c5e0ad8643" />

Images 2. The crafted q parameter is supplied in the browser URL.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b39f05e5-473c-4568-8fc6-3c4054ec02c9" />

Image 3. JavaScript alert displaying 1 executes successfully.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ef7a4797-70e9-4ef0-a665-97a26aa17fd9" />

Image 4. Page source confirms that the injected payload is reflected inside the HTML heading.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/017cfa17-dbb8-4c0c-9bbb-5e9ca68027bf" />

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
