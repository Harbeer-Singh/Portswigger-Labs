# ✅ **PortSwigger Lab: User ID controlled by request parameter — data leakage in redirect**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Insecure Direct Object References / Data leakage in redirects

---

### 📚 **Description**

This lab demonstrates an **access control / data leakage** issue where a user identifier passed as a request parameter influences a redirect URL that leaks sensitive information. The application redirects to a URL containing account-specific data (e.g., an API key) which is derived from a client-controllable `id` parameter. By changing the `id` parameter to another user (such as `carlos`), an attacker can receive a redirect response containing a URL with that victim’s sensitive token.

**Goal:** Manipulate the `id` parameter so the server’s redirect reveals Carlos’s API key (or similar sensitive data) in the `Location` header or redirect target URL, then use that data to complete the lab.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The server constructs a redirect URL using a user-controlled `id` parameter and includes sensitive data in that URL. Because redirects (Location headers) are visible to the client and intermediaries, sensitive information should never be placed into redirect targets. The underlying access control issue is that the server trusts the `id` parameter without verifying the authenticated user’s authorization to access that resource.

#### ✅ Why it's dangerous

* Sensitive tokens or identifiers appearing in redirects can be leaked in browser history, logs, referer headers, or proxy logs.
* An attacker who can control the `id` parameter can force the server to reveal tokens for other users via a redirect, enabling account compromise or data exfiltration.
* Redirect-based leakage is subtle and commonly overlooked during reviews.

#### ✅ Where to look

* Endpoints that perform redirects and include query parameters derived from user input.
* Location headers and redirect targets returned by the server after POST/GET actions.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker logs in and captures a request/response where the server issues a redirect containing a URL built from the `id` parameter.
* Modifying `id` to the victim `carlos` causes the server to generate a redirect whose target URL includes Carlos's API key (or other sensitive token) as part of the path or query string.
* The attacker captures that redirect Location header (or follows the redirect in the browser) and extracts the sensitive data, which can be used to solve the lab or further compromise the account.

**Impact:** Disclosure of sensitive tokens (API keys, session identifiers) through redirect URLs; enables account takeover or unauthorized API access.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker and captured the relevant request/response in Burp Proxy to observe a redirect flow.
2. Identified the `id` parameter in the request that influences the redirect.
3. Modified the `id` parameter value to `carlos` (or Carlos’s identifier) and resent the request via Repeater/Proxy.
4. Observed the server response `302` (or other 3xx) with a `Location` header containing the redirect URL — extracted the API key (or sensitive token) from that URL.
5. Used the revealed API key or token as required by the lab to complete the challenge and confirm access to the victim’s account.

---

### 📸 Screenshots (serially numbered — include these exact filenames in the repo)

![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-7/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-7/images/2.png)

---

### 📝 What I Learned
  
✔ Never embed sensitive tokens or secrets in redirect URLs or Location headers.                         
✔ Always validate that the authenticated user is authorized to access the resource referenced by any client-supplied identifier.                              
✔ Redirect-based leakage is a common, subtle issue — check Location headers and redirect targets during security reviews.                    

---

### 🔐 Mitigation Techniques

✔ Do not include secrets or sensitive tokens in redirect targets — pass references that the server can resolve server-side if needed.                           
✔ Perform strict server-side authorization checks for any request referencing a user-supplied `id`.                         
✔ Use POST-redirect-GET patterns that avoid exposing sensitive data in URLs; store sensitive data server-side and retrieve it after the redirect using a protected request.                               
✔ Log and monitor unusual redirect generation and access patterns.

---

### 🧾 Notes

* The lab requires discovering that the redirect target contains a sensitive token for `carlos` and extracting it from the Location header.
* Only perform such testing in authorized lab environments; in production ensure Location headers never reveal secrets.

---

### 👤 Author

Harbeer-Singh
