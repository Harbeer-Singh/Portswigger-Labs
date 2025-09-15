### ✅ **PortSwigger Lab: Password reset broken logic**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Authentication → Other mechanisms → Resetting user passwords

---

### 📚 **Description**

This lab demonstrates a **broken password reset flow** where the server fails to validate the temporary reset token when updating a password. The token is included in the reset email URL, but the server does not require it when the new password is submitted. This allows an attacker to reuse the password-reset POST request, replace the username with a victim account (e.g., `carlos`), and set an arbitrary password for that account.

**Goal:** Reset Carlos's password using the flawed reset flow, then log in as Carlos and access his "My account" page.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application issues a temporary reset token in the reset email, but the server-side logic does not verify that same token when the password is changed — the token is effectively ignored. This makes the reset form act like a generic password-set endpoint where supplying a username and new password is sufficient to change any user's password.

#### ✅ Why it's dangerous

* Password-reset flows must authenticate the requestor (via a single-use token tied to an email/user) before allowing password changes. If the token is not enforced, attackers can reset other users’ passwords without having access to their email.
* This leads to account takeover with minimal interaction: request reset for any account you control, capture the POST request, modify it for a victim username, and submit.

#### ✅ Where to look

* Reset email generation and whether the reset token is embedded in a URL parameter.
* Server-side handling of password-reset submissions (`POST /forgot-password?...`) and whether the token value is validated before applying the new password.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The reset email contains a URL with `?temp-forgot-password-token=<token>` for the victim or attacker.
* When the user submits the new password via `POST /forgot-password?temp-forgot-password-token=<token>`, the server should validate the token and ensure it matches the intended user. In this lab, the server does not enforce the token on the POST, so removing the token or replacing the `username` field still results in the password being changed.
* By requesting a password reset for an attacker account and replaying the POST with `username=carlos` (and an arbitrary new password), the attacker can set Carlos's password and then log in as him.

**Impact:** Complete account takeover via password reset abuse; a critical authentication logic vulnerability.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker (`wiener:peter`) and clicked "Forgot your password?", submitting the attacker username.
2. Clicked the lab's Email client button to view the password reset email and opened the reset link.
3. In Burp Proxy → HTTP history, located the `POST /forgot-password?temp-forgot-password-token=<token>` request that submits the new password and sent it to Burp Repeater.
4. In Repeater, deleted the value of the `temp-forgot-password-token` parameter (in both the URL and request body) to confirm the server still accepted the request — this confirms the token is not checked.
5. Requested another password reset (to get a fresh POST request), sent it to Repeater, then replaced the `username` parameter with `carlos` and set the `new-password` fields to a chosen password.
6. Sent the modified request; the server accepted the change. Finally, logged in as `carlos` with the new password and clicked "My account" to solve the lab.

---

### 📝 What I Learned

✔ Password reset tokens must be validated server-side when applying a new password; embedding a token in the workflow is useless unless it is enforced.                  
✔ Always verify that password reset forms bind the token to the intended user and that tokens are single-use and expire quickly.                    
✔ Replay and request-reuse techniques are effective for testing reset flows — check both GET and POST handling for token validation.                  

---

### 🔐 Mitigation Techniques
                     
✔ Ensure password reset tokens are single-use, securely generated, tied to a specific user, and validated server-side before applying password changes.                
✔ Do not accept password-change requests without a valid token; reject requests where the token is missing, invalid, or does not match the username.              
✔ Log reset attempts and alert users of password changes; require re-authentication or email confirmation for sensitive account actions.                 
✔ Apply strict rate-limiting to password reset requests to reduce abuse.                            

---

### 🧾 Notes

* Lab-provided attacker credentials: `wiener:peter`. Victim username: `carlos`.
* The lab solution shows that deleting the token or replacing the username in the POST request allows changing another user's password — this demonstrates the token is not validated on submission.
* Only perform such testing in authorized lab environments.

---

### 👤 Author

Harbeer-Singh
