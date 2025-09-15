### ✅ **PortSwigger Lab: Password brute-force via password change**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-brute-force-via-password-change)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Other mechanisms → Changing user passwords

---

### 📚 **Description**

This lab demonstrates a **password brute-force** technique that abuses a flawed password-change workflow. The password-change form leaks whether the supplied current password is correct via subtle differences in returned messages when the new-password fields match vs don't match. By controlling the form (submitting mismatched new-password values) and placing candidate passwords into the `current-password` field for a victim username, an attacker can detect the correct current password without needing to authenticate as that user.

**Goal:** Use the candidate password list to brute-force Carlos's current password via the password-change endpoint and then log in as Carlos to access his "My account" page.

**Lab-provided credentials & targets:**

* Attacker credentials: `wiener:peter`
* Victim username: `carlos`
* Candidate passwords (use the lab-provided list when running the attack).

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The password-change endpoint behaves differently depending on whether the supplied current password is correct. When the supplied `current-password` is correct and the two new-password fields differ, the server returns `New passwords do not match`. When the `current-password` is incorrect (and new-passwords differ), the server returns `Current password is incorrect`. This divergence acts as an oracle allowing an attacker to test candidate passwords for another account.

#### ✅ Why it's dangerous

* Password-change forms that leak the correctness of the current password can be abused to enumerate or brute-force passwords for other accounts.
* Attackers do not need to authenticate as the victim; they can simply submit a crafted request targeting the victim username.
* Once the correct current password is found, the attacker can log in as the victim and fully compromise the account.

#### ✅ Where to look

* Password-change endpoints and form handling logic.
* Differences in error messages or response bodies when submitting password-change requests with mismatched new passwords.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The form includes a hidden `username` field which can be changed to target another user.
* If an attacker submits requests with the two new-password fields intentionally set to different values and cycles through candidate current passwords, the server’s response message reveals whether the current password used was correct.
* Finding the true current password allows the attacker to log in as the victim and access sensitive information.

**Impact:** Account takeover via a logic/response-difference oracle in the password-change flow; high severity for affected accounts.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker (`wiener:peter`) and navigated to the "Change password" page to capture the form behavior.
2. Observed that the form submits a hidden `username` field and returns different messages depending on whether the `current-password` is correct.
3. In Burp Intruder, used the captured `POST /my-account/change-password` request as the attack template, set `username=carlos`, and placed a payload position on the `current-password` parameter.
4. Ensured the two `new-password` fields are set to different values so the server would return `New passwords do not match` only if the supplied current password was correct.
5. Loaded the lab's candidate password list into the payloads panel and started the attack, adding a grep-match for `New passwords do not match` to quickly spot the correct current password.
6. Note the password that triggered the `New passwords do not match` response, then log out and log back in as `carlos` with that password to finish the lab.

---

### 📝 What I Learned

✔ Password-change endpoints must not disclose whether the provided current password is correct via differing error messages.                     
✔ Hidden form fields (like `username`) can be modified by attackers — server-side authorization checks must enforce that the requester is allowed to change the targeted account.                   
✔ When testing for logic bugs, craft requests that intentionally produce ambiguous outputs (e.g., mismatched new passwords) to reveal subtle oracles.                     

---

### 🔐 Mitigation Techniques

✔ Always validate on the server side that the authenticated user is allowed to change the specified account’s password — do not rely on hidden fields.                                
✔ Return identical, generic error messages for failed password-change attempts to avoid leaking information (e.g., "Unable to change password").                   
✔ Rate-limit password-change attempts and monitor suspicious activity.                            
✔ Require re-authentication or multi-factor verification for sensitive account changes.                 

---

### 🧾 Notes

* Lab-provided attacker credentials: `wiener:peter`. Victim username: `carlos`.
* The lab's candidate password list is provided in the lab UI — use it for deterministic reproduction.
* Only perform these techniques in authorized lab environments.

---

### 👤 Author

Harbeer-Singh

