### ✅ **PortSwigger Lab: Offline password cracking**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking](https://portswigger.net/web-security/authentication/other-mechanisms/lab-offline-password-cracking)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Other mechanisms → Offline password cracking

---

### 📚 **Description**

This lab demonstrates **offline password cracking** where the application stores a user's password hash in a cookie (the `stay-logged-in` cookie). The lab also contains a stored XSS in the comment functionality which can be used to exfiltrate another user’s cookie. The goal is to obtain Carlos’s `stay-logged-in` cookie, recover (crack) his password offline, then log in as `carlos` and delete his account from the “My account” page.

**Lab-provided credentials & targets:**

* Attacker credentials: `wiener:peter`
* Victim username: `carlos`

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application stores a predictable `stay-logged-in` cookie encoded as `Base64(username+':' + md5(password))`. If an attacker can obtain that cookie (for example, via stored XSS), they can extract the hash and crack the password offline using a hash database or cracking tool.

#### ✅ Why it's dangerous

* Storing hashes client-side and trusting them for authentication enables offline cracking and cookie forgery.
* XSS can be used to exfiltrate cookies from authenticated users, amplifying the impact of the weak cookie design.
* Once a password is recovered offline, attackers can log in as the victim without further interaction with the site.

#### ✅ Where to look

* Cookie contents, encoding formats, and how the server validates `stay-logged-in` tokens.
* Stored XSS vectors (comment functionality) that can be used to deliver cookie-exfiltration payloads.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker posts a stored XSS payload to a blog/comment which, when viewed by the victim, sends their cookies to the attacker’s exploit server.
* The attacker reads the captured `stay-logged-in` cookie from the exploit server logs, decodes it (Base64 → `carlos:<md5hash>`), and uses offline cracking techniques (hash database or hashcat) or a search engine lookup to recover the plaintext password.
* With the recovered password, the attacker logs in as `carlos`, visits the "My account" page, and deletes the account to complete the lab.

**Impact:** Client-side storage of password-derived secrets and XSS together enable account takeover without ever interacting further with the victim's session.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker (`wiener:peter`) and investigated the "Stay logged in" cookie to confirm it is Base64-encoded and decodes to `username:md5hash`.
2. Located the blog/comment feature and crafted a stored XSS payload that forwards `document.cookie` to the exploit server (the lab provides an exploit server for this purpose).
3. Posted the comment containing the XSS payload and then viewed the exploit server access log to capture the victim's request containing their `stay-logged-in` cookie.
4. Decoded the cookie in Burp Decoder (Base64 → `carlos:<md5hash>`) and extracted the MD5 hash.
5. Used offline cracking techniques to recover the plaintext password for the extracted MD5 hash (the lab demonstrates that searching the hash online reveals the password `onceuponatime`).
6. Logged in as `carlos` using the recovered password and navigated to the "My account" page to delete the account and complete the lab.

---

### 📝 What I Learned
                
✔ Never store password-derived secrets client-side; use server-side tokens or signed, server-validated tokens (HMAC).                
✔ Stored XSS can be combined with weak cookie design to enable powerful account takeover techniques.              
✔ Offline cracking is feasible when hashes are accessible — use slow, salted hashes (bcrypt/Argon2) and never expose hashes to clients.               

---

### 🔐 Mitigation Techniques

✔ Avoid storing password hashes or password-derived values in client-side cookies. Use server-generated random session tokens or signed tokens validated server-side.                   
✔ Fix XSS vulnerabilities in comment or content features by properly encoding/escaping output and implementing Content Security Policy (CSP).                     
✔ Use salted, slow password hashing on the server (bcrypt, scrypt, Argon2) to resist offline cracking.                       
✔ Monitor for unusual exploit-server traffic and log/examine suspicious cookie exfiltration.                

---

### 🧾 Notes

* The lab supplies attacker credentials `wiener:peter` and victim `carlos`.
* The lab solution demonstrates decoding the Base64 cookie and looking up the MD5 hash to find the password `onceuponatime`. In real engagements, use hash-cracking tools or hash databases; avoid submitting real user hashes to public search engines.
* Only perform these techniques in authorized testing environments.

---

### 👤 Author

Harbeer-Singh
