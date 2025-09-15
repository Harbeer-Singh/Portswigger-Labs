### ✅ **PortSwigger Lab: 2FA simple bypass**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Authentication → Multi-factor → 2FA bypass

---

### 📚 **Description**

This lab demonstrates a **simple 2FA bypass** caused by flawed two-factor verification logic. After submitting the first authentication step (username/password), the application does not correctly verify that the same user completes the second step (the 2FA code). This trust/sequence flaw can be abused to access another user’s account without knowing their 2FA code.

**Goal:** Access the account page of the user `carlos` without submitting a valid 2FA code.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application fails to maintain a secure link between the initial authentication step and the second-factor verification, allowing an attacker to complete or skip the second step for another user.

#### ✅ Why it's dangerous

* Two-factor authentication loses its protective value if the second step can be bypassed or completed for a different account.
* Logical/flow vulnerabilities like this are often missed during development and can lead to account takeover even when MFA is enabled.

#### ✅ Where to look

* Session handling and how the server maps the 2FA verification flow to a specific user session.
* Any parameters, cookies, or tokens used between the first and second steps and whether they can be tampered with or replayed.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The server accepts a 2FA completion request without ensuring it pertains to the same user session that performed the initial login step.
* By manipulating the sequence of requests (replaying, forced browsing, or reusing tokens/cookies), an attacker can reach or trigger the post-2FA authenticated state for a victim account.

**Impact:** Attackers can access victim account pages and perform actions without needing the victim's 2FA code.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in with the attacker credentials provided by the lab to observe the normal 2FA flow.
2. Captured the HTTP requests for the first and second authentication steps in Burp Proxy/Repeater.
3. Inspected cookies, session tokens, and any parameters passed between the login step and the 2FA verification step to determine how the server tracks the in-progress authentication.
4. Experimented with replaying or reordering requests (for example, performing the 2FA completion request while logged in as the attacker but swapping or reusing tokens) to see whether the server enforces that the 2FA step matches the same user.
5. Found a sequence that resulted in an authenticated session for the victim `carlos` without supplying his 2FA code — confirmed by visiting the account page.

---

### 📝 What I Learned

✔ Two-factor authentication must be enforced as a flow tied to the original principal — verifying the same user/session throughout.    
✔ Forces like request replay, forced browsing, or session token reuse can expose logic flaws in multi-step authentication.               
✔ Always validate tokens and session state between authentication steps and bind 2FA actions to a server-side session identifier that cannot be tampered with.                                             
                    
---

### 🔐 Mitigation Techniques

✔ Associate 2FA verification tokens with the specific authenticated session/user and validate them server-side before granting the post-2FA authenticated state.                                  
✔ Use short-lived, single-use tokens for the 2FA step and store server-side state that maps token → user.                           
✔ Avoid relying on client-side or guessable session identifiers; treat all steps as part of a single authenticated transaction.          
✔ Log and monitor unusual authentication sequences and require re-authentication for sensitive transitions.             

---

### 🧾 Notes

* The lab supplies attacker credentials `wiener:peter` and victim `carlos:montoya`.
* This lab is classified as *Apprentice* difficulty and demonstrates a common logic-flaw pattern in MFA flows.
* Only perform these techniques in authorized lab environments.

---

### 👤 Author

Harbeer-Singh

