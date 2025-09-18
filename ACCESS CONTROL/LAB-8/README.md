# ✅ **PortSwigger Lab: User ID controlled by request parameter — password disclosure**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Insecure Direct Object References → Sensitive data leakage

---

### 📚 **Description**

This lab demonstrates an **Insecure Direct Object Reference (IDOR)** combined with sensitive data disclosure: the application returns account-specific data including the user’s password when the `id` parameter is set to another user’s identifier. By tampering with the `id` parameter, an attacker can retrieve the victim's password directly from the server response.

**Goal:** Modify the request `id` parameter to retrieve Carlos’s account page and discover his plaintext password, then log in as `carlos` and access the "My account" page.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

A request parameter controls which user account is returned, and the server includes sensitive information (the user’s password) in the response. The server fails to verify that the authenticated user is allowed to view the requested account, resulting in direct disclosure of secrets.

#### ✅ Why it's dangerous

* Exposing plaintext passwords (or other secrets) in any response is a critical vulnerability.
* Combining IDOR and data leakage enables trivial account compromise without brute-forcing or social engineering.
* Such flaws can lead to rapid account takeover and lateral movement if reused credentials exist.

#### ✅ Where to look

* Account/profile endpoints, API calls that return user objects, and any response bodies that include password or credential fields.
* Hidden/implicit `id`, `user`, or `userid` parameters that influence which account is returned.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker intercepts a request to their own account page and identifies the `id` parameter.
* By replacing the `id` value with Carlos’s identifier and resending the request, the server responds with Carlos’s account page containing his plaintext password in the response HTML.
* The attacker uses that password to log in as Carlos and completes the lab by visiting the "My account" page.

**Impact:** Immediate account takeover risk and severe confidentiality breach — critical severity.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker and viewed the account page to capture the request in Burp Proxy.
2. Located the `id` parameter in the request URL or body that controls which user account the server returns.
3. Modified the `id` parameter to Carlos’s identifier and resent the request via Repeater/Proxy.
4. Examined the server response and located Carlos’s plaintext password in the HTML.
5. Used the discovered password to log in as Carlos and navigated to the "My account" page to confirm access and finish the lab.

---

### 📸 Screenshots

![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-8/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-8/images/2.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-8/images/3.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-8/images/4.png)

---

### 📝 What I Learned

✔ Never return plaintext passwords or secret data in responses — store and handle credentials securely using best-practice hashing and never expose them.                      
✔ Server-side authorization must be enforced for any request that returns user-specific data.                                   
✔ IDORs combined with data leakage are extremely high-impact vulnerabilities — include OWASP Top 10-related checks in code reviews.                                         

---

### 🔐 Mitigation Techniques

✔ Remove sensitive fields (passwords, secrets) from API responses and HTML pages; never include them client-side.                   
✔ Enforce object-level authorization checks on every request that references a user-provided identifier.                           
✔ Store passwords using strong salted hashing (bcrypt/Argon2) and never in plaintext.                       
✔ Implement logging and alerting for abnormal access to account endpoints and unusual parameter tampering.                            

---

### 🧾 Notes

* The lab requires replacing the `id` parameter with Carlos’s identifier to view his password in the response, then using it to log in.
* Only perform these actions in authorized lab environments; in production, ensure passwords are never disclosed.

---

### 👤 Author

Harbeer-Singh

