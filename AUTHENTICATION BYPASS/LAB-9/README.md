
### ✅ **PortSwigger Lab: 2FA bypass using a brute-force attack**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-bypass-using-a-brute-force-attack](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-bypass-using-a-brute-force-attack)

## ⚙️ **Difficulty:** EXPERT

📂 **Category:** Authentication → Multi-factor → 2FA brute-force

---

### 📚 **Description**

This lab demonstrates bypassing two-factor authentication (2FA) by brute-forcing the 4-digit verification code. You are given a valid username and password for the victim (`carlos:montoya`) but do not have the 2FA code. The verification code is short (4 digits), so with careful automation and session handling (to re-login when necessary) it can be brute-forced.

The lab highlights practical challenges such as the verification code resetting during the attack and the need to automatically re-establish authenticated state between attempts.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

A short numeric 2FA code (4 digits) is susceptible to brute-force. The application requires re-authentication after a couple of failed attempts, so successful brute-forcing requires automating the login flow before each guess (e.g., using Burp macros or Turbo Intruder).

#### ✅ Why it's dangerous

* Short, low-entropy 2FA codes are vulnerable to brute-force attacks.
* Without protections like per-account rate-limiting, throttling, or single-use code constraints, attackers can eventually guess the correct code.
* Automated session handling is required to maintain the test harness during the attack, adding operational complexity but not preventing success.

#### ✅ Where to look

* 2FA verification endpoints (`POST /login2`) and session handling behavior.
* Any server-side protections (rate-limits, failed-attempt counters) and whether they require re-authentication.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The server accepts 4-digit verification codes and will reset or invalidate codes at intervals; sometimes the code resets while the automated attack is running.
* To brute-force effectively, the attack must re-login as the victim before each `POST /login2` attempt — this is achieved by using a Burp Session Handling Rule with a macro that performs the login sequence.
* Running Intruder with a Numbers payload (0000–9999), a resource pool limited to a single concurrent request, and the macro in place allows systematic brute-forcing until a `302` success is observed.

**Impact:** Short MFA codes without additional protections can be brute-forced, enabling account takeover even when MFA is in place.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Confirmed the victim credentials provided by the lab (`carlos:montoya`) and captured the normal login + 2FA requests in Burp.
2. Created a Session Handling Rule in Burp that runs a macro to perform the login sequence automatically before each Intruder request. The macro recorded the `GET /login`, `POST /login`, and `GET /login2` sequence.
3. Sent the `POST /login2` request to Burp Intruder, added a payload position for the `mfa-code` parameter, and configured a Numbers payload for 0000–9999 (4-digit range).
4. Added the attack to a resource pool with the Maximum concurrent requests set to `1` to ensure sequential attempts.
5. Started the attack and monitored responses — one request eventually returned a `302` status indicating successful verification.
6. Right-clicked that request, opened it in the browser, clicked “My account”, and confirmed access to Carlos's account page to complete the lab.

---

### 📝 What I Learned

✔ Short numeric 2FA codes are inherently weak — increase entropy or use time-based OTPs with protections.                         
✔ Session handling automation (macros) is often necessary to brute-force flows that require re-authentication.            
✔ Rate-limiting and single-use code constraints help mitigate brute-force attacks against 2FA.           

---

### 🔐 Mitigation Techniques

✔ Use longer, higher-entropy 2FA tokens (TOTP, U2F) and avoid short numeric codes where possible.                                
✔ Enforce strict rate-limiting or progressive delays on 2FA verification attempts.                
✔ Bind verification codes to single-use server-side tokens and invalidate codes immediately after a small number of failed attempts.     
✔ Monitor and alert on automated verification attempts and require multi-step throttling.                   

---

### 🧾 Notes

* Victim credentials (from the lab): `carlos:montoya`.
* The lab suggests using Burp macros or Turbo Intruder; the macros + Intruder Numbers payload approach is a reliable method for this lab.
* The verification code may reset during the attack — you may need to re-run the attack or adjust timing accordingly.

---

### 👤 Author

Harbeer-Singh
