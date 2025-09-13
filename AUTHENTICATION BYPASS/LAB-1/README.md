# ✅ **PortSwigger Lab: Username enumeration via different responses**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)

## ⚙️ **Difficulty:** Medium

📂 **Category:** Authentication → Password-based → Username enumeration

---

### 📚 **Description**

This lab demonstrates **username enumeration** where the application leaks whether a username exists by returning different responses (message content, subtle HTML differences, or status/length variations) for valid vs invalid usernames during an authentication-related flow (login or password-reset).
The goal was to determine at least one valid username on the target application by observing and comparing responses for different username inputs.

This vulnerability arises when the application reveals account existence through distinct responses (e.g., “Unknown user” vs “Check your email”, additional HTML elements, or measurable length differences). Even minor differences let attackers enumerate accounts, which can be used for phishing, credential stuffing, targeted attacks, or social engineering.

---

### ⚡ **Key Takeaways**

#### ✅ What is Username Enumeration?

Username enumeration is when an attacker can determine whether a username/account exists by sending crafted requests and observing differences in server responses.

#### ✅ Why it's dangerous

* Confirms valid accounts for targeted attacks (phishing, brute-force, credential stuffing).
* Amplifies impact of leaked credential databases.
* Often combined with weak password policies to gain unauthorized access.

#### ✅ Vulnerable Endpoints / Parameters

* `username` or `email` fields in login, password-reset, or account-recovery forms.
* Any endpoint that returns different messages, status codes, or subtle HTML/text differences based on account existence.

---

### ⚙️ **Explanation of Observed Differences and Impact**

* The application responded differently when submitting a password-reset or login request depending on whether the supplied username existed.
* Example differences: distinct success/failure message text, presence/absence of an extra HTML element, or slight response-length variation.
* These deterministic differences allowed confirming valid usernames by comparing pairs of requests (one with a candidate username, one with a known-nonexistent username) and spotting consistent divergences.

**Impact:** An attacker can harvest valid accounts and then run targeted attacks (credential stuffing, password spraying), making this a high-impact information disclosure even if the vulnerability does not directly reveal passwords.

---

### 🧪 Methodology — How I approached it

1. Opened the login / password reset page and inspected the form.
2. Identified where `username` (or `email`) was submitted and captured the request in Burp.
3. Sent requests with a definitely-invalid username (e.g., `no-such-user-12345`) and recorded the full response body and status.
4. Sent requests with candidate usernames (e.g., `administrator`, `harbeer`, etc.) and compared responses for differences (text, elements, length, status, headers).
5. Automated (carefully, rate-limited) the comparison where needed to enumerate multiple candidates.

---

### 📸 Screenshots (serially numbered — include these exact filenames in the repo)

1. **Request (invalid username) — intercepted in Burp**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-1/images/1.png)

2. **Response (invalid username) — completed response showing error message**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-1/images/1.png)

3. **Request (valid username) — intercepted in Burp**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-1/images/1.png)

4. **Response (valid username) — completed response showing success/next-steps message**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-1/images/1.png)

5. **Burp Comparer / Response diff** — highlights the HTML/text differences used to confirm enumeration.
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-1/images/1.png)

---

### 📝 What I Learned

✔ Small textual or structural differences in responses can be powerful signals to enumerate accounts.                   
✔ Always test multiple vectors: login, password-reset, registration, and any AJAX endpoints.                         
✔ Use automated, rate-limited scripts or Burp Intruder with safe delays to enumerate multiple usernames ethically in lab environments.   
✔ Defensive coding should assume attackers will compare responses — do not leak existence information.                 
✔ Combining enumeration results with password lists substantially increases attack success chance.                         

---

### 🔐 Mitigation Techniques

✔ Return identical responses for both existent and non-existent accounts in authentication and recovery flows (e.g., “If the account exists, we will send instructions”).                                                    
✔ Use same HTTP status codes and response shapes for both cases.                    
✔ Avoid revealing timing differences — add uniform processing delays where appropriate (but avoid relying solely on timing).                      
✔ Rate-limit or throttle account-recovery attempts and log suspicious activity.                      
✔ Implement multi-factor authentication to reduce value of enumerated usernames.                         
✔ Monitor for automated enumeration patterns and block abusive IPs or require CAPTCHA for suspicious flows.                   

---

### 🧾 Notes

* During testing I used Burp Suite to capture requests/responses and to perform diffs.
* When enumerating usernames at scale, respect rate-limits and use randomized delays to avoid tripping protections (in labs keep it reasonable to demonstrate the point).

---

### 👤 Author

Harbeer-Singh
