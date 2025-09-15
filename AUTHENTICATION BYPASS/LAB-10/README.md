### ✅ **PortSwigger Lab: Brute-forcing a stay-logged-in cookie**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/other-mechanisms/lab-brute-forcing-a-stay-logged-in-cookie](https://portswigger.net/web-security/authentication/other-mechanisms/lab-brute-forcing-a-stay-logged-in-cookie)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Authentication → Other mechanisms → Keeping users logged in

---

### 📚 **Description**

This lab demonstrates brute-forcing a poorly-designed “stay logged in” cookie. The cookie encodes the username and a hash of the password and is trivially reproducible. By constructing candidate cookie values from guessed passwords (hash → prefix with username → Base64), an attacker can find a cookie that allows them to access another user’s account page.

**Goal:** Brute-force Carlos's `stay-logged-in` cookie and access his My account page.

**Lab-provided credentials & hints:**

* Attacker credentials: `wiener:peter`
* Victim username: `carlos`
* The lab suggests the cookie is `base64(username + ':' + md5(password))` and recommends using the candidate password list provided by the lab.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application stores a “stay-logged-in” cookie that contains the username and an MD5 hash of the password, Base64-encoded. Because the format is predictable and MD5 is fast and reversible to brute-force via dictionary attacks, an attacker can construct valid cookies for other users by hashing candidate passwords.

#### ✅ Why it's dangerous

* Encoding secrets client-side without a secure MAC or server-side binding lets attackers impersonate users.
* MD5 is fast and not collision-resistant; without salting or server-side verification beyond matching the hash, cookies are brute-forceable.
* An attacker with a list of candidate passwords can rapidly test many possibilities and identify a matching cookie.

#### ✅ Where to look

* Cookie contents and how they’re generated/validated.
* Presence of predictable formats like `username:hash` and whether the server trusts the cookie blindly.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The cookie can be decoded (Base64) to reveal the username and an MD5 hash.
* Reproducing the cookie for a target user requires hashing candidate passwords with MD5, prefixing the username and a colon, then Base64-encoding the result.
* Submitting the generated cookie in a request for the target user’s account page yields access when a candidate password matches, indicated by the account page content (e.g., the presence of the "Update email" button).

**Impact:** Account takeover via forged persistent cookies — a severe authentication bypass that negates the protection of password-based authentication.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker (`wiener:peter`) with the "Stay logged in" option enabled and observed the `stay-logged-in` cookie in Burp’s Inspector.
2. Decoded the cookie (Base64) and verified it contained `wiener:<md5hash>`. Confirmed hashing the attacker password with MD5 produced the observed hash.
3. Sent a `GET /my-account?id=wiener` request to Intruder and noted that Burp marked the `stay-logged-in` cookie as a payload position.
4. In Intruder, used the candidate password list as payloads and applied the following payload-processing rules (so each password guess is transformed into a cookie value):

   * Hash the payload with MD5.
   * Add the prefix `wiener:` (for initial testing) or `carlos:` when attacking the victim.
   * Base64-encode the final string.
5. Added a grep-match rule to flag responses containing the string `Update email`, which indicates a successful account page load.
6. Verified the pipeline by including the attacker’s own password and confirming a successful request loaded the attacker account page.
7. Replaced the payload list with the lab’s candidate passwords, changed the prefix to `carlos:`, and re-ran the attack against `GET /my-account?id=carlos`.
8. When the attack returned a response containing `Update email`, the payload that generated that cookie was the correct password-derived cookie for Carlos and the lab was solved.

---

### 📸 Screenshots (serially numbered — include these exact filenames in the repo)

1. **Simple List Attack**  
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-10/images/1.png)

2. **Completed**  
   ![Modified Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/AUTHENTICATION%20BYPASS/LAB-10/images/2.png)

---

### 📝 What I Learned

✔ Never trust client-side tokens for authentication without server-side binding and integrity checks (use HMACs or signed tokens).                       
✔ Storing username + hash directly in a cookie allows trivial reproduction and brute-forcing.                      
✔ Fast unsalted hashes (MD5) are poor choices for secrets used in client-side tokens.                
✔ Use server-side validation, unpredictable tokens, and short expiry for persistent login mechanisms.             

---

### 🔐 Mitigation Techniques

✔ Do not store `username:hash(password)` in client-side cookies. Instead, use server-generated random tokens or signed tokens (HMAC) with server-side lookup.                
✔ Use slow, salted password hashing for stored passwords (bcrypt/Argon2) and never expose raw hash values to clients.                         
✔ Limit the usefulness of persistent login cookies by binding them to device fingerprints and IPs, and allow easy revocation server-side.                          
✔ Monitor for suspicious cookie-based authentication events and require re-authentication for sensitive actions.                        
                  
---

### 🧾 Notes

* Lab-provided attacker credentials: `wiener:peter`. Victim: `carlos`. Candidate passwords are provided by the lab.
* The lab suggests verifying cookie construction by decoding the attacker cookie and hashing the attacker password with MD5 to match the observed hash.
* Only perform these attacks in authorized lab environments.

---

### 👤 Author

Harbeer-Singh
