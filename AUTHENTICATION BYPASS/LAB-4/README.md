# ✅ **PortSwigger Lab: Broken brute-force protection, IP block**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/password-based/lab-broken-bruteforce-protection-ip-block](https://portswigger.net/web-security/authentication/password-based/lab-broken-bruteforce-protection-ip-block)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Password-based → Brute-force protections

---

### 📚 **Description**

This lab demonstrates a **broken brute-force protection** mechanism where the application temporarily blocks IPs after a number of failed login attempts, but the protection can be bypassed via a logic flaw. The attacker’s goal is to brute-force the victim’s password while evading the IP block and then log in to access the victim’s account page.

The lab illustrates how flawed rate-limiting logic or improper session/account state handling can be used by an attacker to reset or circumvent account/IP-based protections.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application blocks IPs after a small number of failed login attempts. However, the block counter can be reset by performing a successful login with the attacker’s own account before the limit is reached, allowing the attacker to interleave attempts and eventually brute-force the victim’s password.

#### ✅ Why it's dangerous

* Allows an attacker to perform large-scale password guessing attacks despite naive IP-based blocking.
* Logical flaws in how login failures are tracked (per-IP vs per-account) can be exploited to bypass protections.
* Once the correct password is found, account takeover is possible.

#### ✅ Where to look

* Login endpoint (`POST /login`) and any logic that increments or resets failed-login counters.
* How the application tracks attempts (IP address, username, session) and whether it trusts headers or allows counter resets under certain conditions.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The application increments a counter for failed login attempts and temporarily blocks further attempts from that IP after a threshold (e.g., 3 failed attempts).
* A logic flaw allows an attacker to reset this counter by logging in with their own valid credentials before the threshold is hit, or by interleaving requests between the attacker and victim username in a crafted payload sequence.
* Using a carefully crafted Pitchfork-style Intruder attack (alternating attacker and victim usernames and synchronizing passwords) with a single concurrent request (resource pool) ensures requests are processed in order and the brute-force can proceed without tripping the block.

**Impact:** This enables the attacker to discover the victim’s password and access their account despite the presence of a naive brute-force protection mechanism.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Inspected the login page and confirmed that repeated failed login attempts lead to an IP-based block after a small number of tries.
2. Confirmed that logging in with a valid account resets or influences the failed-attempt counter.
3. Configured Burp Intruder with a Pitchfork-style attack where payload list 1 alternates between the attacker username and the victim username, and payload list 2 contains candidate passwords aligned with the usernames.
4. Added the attack to a resource pool with `Maximum concurrent requests = 1` to ensure requests are sent sequentially and in the expected order.
5. Launched the attack and, after completion, filtered results to find the single `302` redirect corresponding to the successful login for the victim username.
6. Logged in to the victim’s account using the discovered password to access the account page and complete the lab.

---

### 📸 Screenshots 



---

### 📝 What I Learned

✔ IP-based blocking is not sufficient if the logic to track failures can be reset or manipulated.                  
✔ Interleaving attacker and victim requests in a controlled sequence can bypass naive brute-force protections.                 
✔ Using resource pools and single-concurrency attacks ensures ordered request delivery when the server’s stateful logic relies on sequence.                
✔ Always test both successful and failed login flows to understand how counters and blocks behave.                   

---

### 🔐 Mitigation Techniques

✔ Track failed login attempts per account and per IP, and use conservative strategies that combine both signals.                   
✔ Avoid designs that allow easy resetting of failure counters through unrelated successful logins.                  
✔ Implement progressive delays, account lockouts (with safe recovery), and multi-factor authentication.                   
✔ Monitor login attempts and block suspicious sequences, not just simple per-IP counters.                         

---

### 🧾 Notes

* The lab provides the attacker credentials `wiener:peter` and the victim username `carlos` to solve the lab.
* The lab hint suggests using a macro or Turbo Intruder for advanced users, but the described Pitchfork + resource-pool approach works without them.
* Always perform such attacks only in authorized, lab environments.

---

### 👤 Author

Harbeer-Singh

