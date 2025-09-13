# ✅ **PortSwigger Lab: Username enumeration via response timing**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-response-timing](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-response-timing)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Password-based → Username enumeration

---

### 📚 **Description**

This lab demonstrates **username enumeration using response timing**. The application leaks whether a username exists through measurable differences in response times: responses for valid usernames take noticeably longer depending on the password length, while invalid usernames return with roughly constant (shorter) response times. The lab includes IP-based brute-force protections that can be bypassed by spoofing the `X-Forwarded-For` header.

**Goal:** Enumerate a valid username via timing analysis, then brute-force that user’s password and access the account page.

---

### ⚡ **Key Takeaways**

#### ✅ What to look for

* Timing side-channels where valid usernames induce slower responses (e.g., server performs an expensive operation when the username exists).
* Small, consistent differences in `response received` vs `response completed` times across repeated requests.
* Presence of IP-based protections that can be evaded by header manipulation (e.g., `X-Forwarded-For`).

#### ✅ Why it's dangerous

* Timing leaks enable attackers to enumerate valid usernames without obvious textual differences in responses.
* Once a username is found, password brute-forcing becomes much more efficient.
* Header-based protections (IP checks) may be bypassed if the application trusts forwarding headers.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The server performs additional processing for valid usernames (for example, variable-time password checks or other logic), causing the response to complete more slowly when the username exists.
* Attackers can automate login attempts across candidate usernames, measure response timings, and identify the username(s) that consistently take longer.
* After identifying a valid username, a password attack (with IP spoofing via `X-Forwarded-For` to evade rate limits) reveals the correct password when one of the attempts yields a redirect or success indicator.

**Impact:** Confidential user accounts can be discovered and compromised, exposing personal data or allowing account takeover.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Captured a sample login request in Burp to build a baseline and confirm the application’s IP-based brute-force protection.
2. Verified the application supported the `X-Forwarded-For` header to spoof the source IP and bypass IP-based rate-limiting.
3. Sent login attempts with a variety of candidate usernames (using the lab's username list), keeping the password field set to a long string to amplify timing differences.
4. Configured Burp Intruder with a **Pitchfork** attack — payload positions were the spoofed IP header and the `username` parameter, and a numeric payload (range) was used to vary the IP.
5. Enabled timing-related result columns (Response received and Response completed) and ran the attack.
6. Sorted and reviewed results to find usernames that consistently had longer response times.
7. Created a follow-up Intruder attack using the identified username and the candidate password list (again spoofing IP via `X-Forwarded-For`).
8. Located the request that returned a `302` status (login success) and used those credentials to log in and access the account page.

---

### 📸 Screenshots (serially numbered — include these exact filenames in the repo)

1. **Pitchfork Attack (Username)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-3/images/1.png)

2. **Response (Username Found)**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-3/images/2.png)

3. **Pitchfork Attack (password)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-3/images/3.png)

4. **Response (Password Found)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-3/images/4.png)

5. **Complete**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-3/images/5.png)

---

### 📝 What I Learned

✔ Response-time side-channels are subtle but powerful — they can leak account existence even when textual responses are identical.       
✔ Combining timing analysis with header spoofing can bypass naive rate-limiting defenses.                                
✔ Use Intruder’s timing columns and repeat tests to ensure signals are consistent and not noise.                        
✔ Always perform these techniques only in authorized testing environments.                      

---

### 🔐 Mitigation Techniques

✔ Ensure authentication code paths take a constant amount of time regardless of whether a username exists (constant-time comparisons and uniform control flow).                                        
✔ Do not rely solely on forwarding headers for IP-based protections; validate and control which proxies are trusted.                     
✔ Implement robust rate-limiting and anomaly detection that can identify header-manipulation patterns.                       
✔ Use multi-factor authentication to limit the impact of compromised credentials.                      

---

### 🧾 Notes

* The lab provides candidate username and password lists and the credentials `wiener:peter` as part of the lab guidance.            
* In this lab a Pitchfork-style Intruder attack combined with `X-Forwarded-For` spoofing was an efficient way to spot timing differences and then brute-force the password.                         
* Always run timing tests multiple times to reduce false-positives due to network jitter.                    
 
---

### 👤 Author

Harbeer-Singh
