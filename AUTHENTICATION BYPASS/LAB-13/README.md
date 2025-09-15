### ✅ **PortSwigger Lab: Password reset poisoning via middleware**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware)

## ⚙️ **Difficulty:** PRACTITIONER

📂 **Category:** Authentication → Other mechanisms → Resetting user passwords

---

### 📚 **Description**

This lab demonstrates **password reset poisoning** caused by trusting the `X-Forwarded-Host` header (middleware misconfiguration). The application generates password-reset links using a host value derived from incoming headers; an attacker can supply a custom `X-Forwarded-Host` to make the reset link point to an attacker-controlled domain (the exploit server). When the victim clicks the poisoned link in their email, the attacker can capture the token and use it to reset the victim's password.

**Goal:** Steal the reset token for `carlos` by poisoning the reset link, then use the stolen token to set a new password and log in as Carlos.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

Middleware or application logic trusts the `X-Forwarded-Host` header when building absolute links in emails. An attacker can request a reset for the victim while setting `X-Forwarded-Host` to point at their exploit server; the victim’s email will contain a link to the attacker-controlled domain that includes the valid reset token.

#### ✅ Why it's dangerous

* Poisons email links and allows token theft without access to the victim’s mailbox.
* Trusting client-controllable host headers is a common middleware misconfiguration that leads to token leakage.
* Once the token is stolen, password reset flows that accept the token allow account takeover.

#### ✅ Where to look

* Password reset email generation and how absolute URLs are constructed.
* Whether the app trusts `X-Forwarded-Host`, `Host`, or other forwarded headers from proxies.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* Submitting the password-reset request with `X-Forwarded-Host: <your-exploit-server>` causes the server to include a reset link pointing at the exploit server in the email sent to the victim.
* When Carlos clicks the poisoned link, the exploit server receives a `GET` containing the `temp-forgot-password-token` query parameter. The attacker copies that token and pastes it into the legitimate reset URL (on the real site) to set a new password for Carlos.

**Impact:** Account takeover via token theft resulting from trusting client-supplied host headers — a critical authentication weakness.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker (`wiener:peter`) and captured the `POST /forgot-password` request in Burp.
2. Confirmed that adding an `X-Forwarded-Host` header is supported by the application.
3. Noted the exploit server URL shown in the lab UI and set `X-Forwarded-Host` to `YOUR-EXPLOIT-SERVER-ID.exploit-server.net` in the `POST /forgot-password` request, then changed `username=carlos` and sent the request.
4. Opened the exploit server access log — observed a `GET /forgot-password` request from the victim that included the `temp-forgot-password-token` query parameter.
5. Copied the stolen token and pasted it into the legitimate reset URL on the real site (replace `temp-forgot-password-token` value).
6. Loaded the legitimate reset page with the stolen token and set a new password for Carlos.
7. Logged in as Carlos with the newly-set password to complete the lab.

---

### 📝 What I Learned

✔ Do not trust client-controlled headers when constructing absolute links in emails.                  
✔ Exploit servers are useful in lab environments to demonstrate header-based poisoning attacks safely.              
✔ Always validate and canonicalize host values server-side, and prefer generating links using known server config, not request headers.                 

---

### 🔐 Mitigation Techniques

✔ Ignore or validate forwarded host headers — only accept them from trusted reverse proxies.                           
✔ Construct absolute links using server-side configuration (configured base URL) rather than client-provided headers.               
✔ Use single-use, short-lived reset tokens and verify them against the intended recipient and user.                     
✔ Monitor and alert on suspicious password-reset activity and unexpected outgoing email link domains.             

---

### 🧾 Notes

* Lab-provided attacker credentials: `wiener:peter`. Victim: `carlos`. The exploit server URL is shown in the lab UI.
* The lab solution demonstrates adding `X-Forwarded-Host` and reading the token from the exploit server logs — this is a middleware misconfiguration exploit.
* Only perform these techniques in authorized lab environments.

---

### 👤 Author

Harbeer-Singh

