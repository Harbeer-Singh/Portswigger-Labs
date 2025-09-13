# ✅ **PortSwigger Lab: Username enumeration via subtly different responses**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Password-based → Username enumeration

---

### 📚 **Description**

This lab is subtly vulnerable to **username enumeration** and subsequent password brute-forcing. The target contains an account with a predictable username and password that can be discovered using the provided candidate wordlists. The goal is to first enumerate a valid username by detecting a subtle difference in the application’s responses, then brute-force that user’s password to access their account page.

The vulnerability arises because the application returns a nearly identical error message for failed logins, but one response differs in a tiny way (a trailing space / typo). Automated comparison (e.g., Burp Intruder with a Grep - Extract) exposes that subtle signal, which can be used to identify a valid username.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

A subtle, deterministic difference in the server's response for a specific username leaks account existence. Even tiny variations (a trailing space, punctuation, or a single-character typo) are enough to distinguish valid from invalid accounts when using automated analysis.

#### ✅ Why it's dangerous

* Confirms valid usernames that can be targeted with password attacks (brute-force, credential stuffing).
* Subtle differences are easy to miss manually but are trivial for automated tools, increasing the risk of large-scale enumeration.
* Once a valid username is known, it significantly reduces the effort to gain unauthorized access.

#### ✅ Where to look

* Login forms (`POST /login`) and any authentication-related endpoints.
* Response text, status codes, headers, and body lengths.
* Use automated comparison (Intruder, grep/extract, diff tools) to detect subtle differences.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* Using an automated payload list of candidate usernames, the attacker submits multiple login attempts and extracts a specific error message from each response.
* Sorting the Intruder results by the extracted text reveals one response that is slightly different (contains a trailing space/typo).
* That username is likely valid. The attacker then targets that username with a password list; a successful password results in a `302` redirect indicating login success.

**Impact:** An attacker can obtain valid credentials for an account and access the user's account page, potentially exposing sensitive data or performing actions on behalf of the user.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Opened the login page and captured a sample login request in Burp.
2. Sent a request with a known-invalid username to establish a baseline response.
3. Sent the request to Burp Intruder and configured the `username` field as the payload position.
4. Loaded the candidate username list provided by the lab as the payloads.
5. In Intruder → Settings, used **Grep - Extract** to extract the error message text from responses.
6. Ran the attack, then sorted the results by the extracted column to spot the one subtly different response (the typo/trailing space).
7. Noted the identified username and switched the payload position to the `password` parameter.
8. Used the candidate password list and launched a password attack against the identified username.
9. Observed one request returning a `302` redirect (login success).
10. Logged in with the discovered credentials and opened the account page to complete the lab.

---

### 📸 Screenshots 

1. **Simple list Attack (Username)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-2/images/1.png)

2. **Response (Username Found)**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-2/images/2.png)

3. **Simple list Attack (password)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-2/images/3.png)

4. **Response (Password Found)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-2/images/4.png)

5. **Complete**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-2/images/5.png)

---

### 📝 What I Learned

✔ Tiny, deterministic differences in error messages can completely undermine attempts to hide account existence.                    
✔ Automated tools (Intruder with Grep - Extract or custom scripts that compare response bodies) are invaluable for detecting subtle signals.                  
✔ Enumerate usernames first — it’s usually faster and more effective than trying a full credential cluster bomb.               
✔ After finding a valid username, targeted password attacks (with rate-limiting and monitoring awareness in mind) are more efficient.   

---

### 🔐 Mitigation Techniques

✔ Ensure authentication failure responses are identical for both existing and non-existing accounts (same text, HTTP status, and response structure).                               
✔ Avoid accidental typos or differences between error messages — treat response text as sensitive.                 
✔ Throttle and rate-limit login attempts and implement multi-factor authentication.                         
✔ Monitor for automated enumeration patterns and use CAPTCHA or progressive delays for suspicious flows.                 

---

### 🧾 Notes

* The lab provides candidate username and password wordlists — use them for deterministic results.
* In the lab the subtle signal was a trailing space / typo in the error message. In real-world testing, look for any consistent signal — whitespace, punctuation, additional HTML, or length differences.
* Always perform automated attacks responsibly and only in authorized testing environments (like this lab).

---

### 👤 Author

Harbeer-Singh

