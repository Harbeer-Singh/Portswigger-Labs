# ✅ **PortSwigger Lab: Username enumeration via account lock**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-account-lock](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-account-lock)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Password-based → Username enumeration / Account locking

---

### 📚 **Description**

This lab demonstrates **username enumeration via account lock** logic flaws. The application implements account locking after multiple failed login attempts, but the lock behavior leaks account existence: by repeating username submissions a small number of times, certain usernames trigger a different error message (`You have made too many incorrect login attempts.`). The goal is to identify a valid username using this signal, then brute-force that user’s password and access their account page.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

Account-locking logic introduces a detectable state change. When a username reaches the failed-attempt threshold, the application returns a distinct error message. That message acts as an oracle to confirm a username exists.

#### ✅ Why it's dangerous

* Attackers can enumerate valid usernames without textual differences in regular login responses by deliberately triggering account locks and looking for the lock message.
* Once a valid username is known, password attacks become feasible and more targeted.
* Rate-limiting or lockout mechanisms can inadvertently leak sensitive information if their stateful behavior differs between accounts.

#### ✅ Where to look

* Login endpoints and any response that changes after repeated failures.
* Error messages and response lengths after sending multiple attempts for the same username.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* Submitting the same username multiple times (e.g., repeating each candidate 5 times) causes the server to eventually return the account-lock message for valid usernames.
* By running a Cluster Bomb-style attack that repeats usernames, the attacker can detect which username produced the lock message and mark it as valid.
* After identifying the username, a focused password attack (Sniper-style) with a password wordlist reveals the correct password; following a short wait for the account lock to expire, the attacker can log in and access the account page.

**Impact:** Attackers can enumerate accounts and take over them despite the presence of lockout protections, due to the protections themselves leaking information.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Captured a sample `POST /login` request in Burp to use as the attack template.
2. Configured an Intruder attack that repeats each candidate username multiple times (the lab solution uses a Cluster Bomb approach with the username payload and a second null payload that repeats entries 5 times).
3. Ran the attack and inspected responses — one username's responses contained a different error message (`You have made too many incorrect login attempts.`) and/or consistently longer response times.
4. With the identified username, launched a second Intruder attack against the `password` parameter using the lab's password list (Sniper mode) and a grep-extract for error messages to speed identification.
5. Found the password that produced a response lacking the usual error message (indicating success).
6. Waited the short lockout window to let the account unlock, then logged in with the discovered credentials and visited the account page to complete the lab.

---

### 📸 Screenshots (serially numbered — include these exact filenames in the repo)

1. **Simple list Attack (Username)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-5/images/1.png)

2. **Setting up a macro**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-5/images/2.png)

3. **Setting max requests limit**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-5/images/3.png)

4. **Response (Password Found)**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-5/images/4.png)

5. **Complete**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-5/images/5.png)

---

### 📝 What I Learned

✔ Account-lock or rate-limiting controls can become oracles when their stateful behavior differs between accounts.               
✔ Repeating candidates and analyzing variations in responses (message text, length, timing) can reveal valid usernames.                
✔ After identifying usernames, targeted password attacks are more effective — but account locks may require waiting or coordination.               
✔ Always test defenses for information leakage as well as prevention.                         

---

### 🔐 Mitigation Techniques

✔ Ensure lockout responses are indistinguishable from generic failure messages (avoid revealing lock status).                            
✔ Consider incremental delays or progressive throttling instead of binary locks that reveal state changes.                
✔ Track attempts using combined signals (IP, account, session) and ensure protections do not leak account existence.                 
✔ Log suspicious sequences and require multi-factor authentication for sensitive operations.        

---

### 🧾 Notes

* The lab provides candidate username and password lists — use them to replicate deterministic results.      
* The lab solution suggests repeating each username multiple times (5) to trigger the lock message for valid accounts.               
* Only perform these tests in authorized lab environments.         
 
---

### 👤 Author

Harbeer-Singh

