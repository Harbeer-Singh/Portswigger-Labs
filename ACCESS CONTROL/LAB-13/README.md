# ✅ **PortSwigger Lab: Referer-based access control**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-referer-based-access-control](https://portswigger.net/web-security/access-control/lab-referer-based-access-control)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Referer-based protections → Authorization bypass

---

### 📚 **Description**

This lab demonstrates the weaknesses of **Referer-based access control**, where the application relies on the `Referer` header to determine whether a request should be allowed to perform privileged actions. The goal is to bypass the Referer check and perform an admin-only action (delete the user `carlos`) by sending requests with a forged or modified `Referer` header.

Referer-based checks are brittle because the `Referer` header can be controlled or spoofed by an attacker (via a proxy or interceptor), and some browsers or proxies may omit or modify the header. Authorization must not rely on the presence or value of `Referer`.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The server permits privileged operations when the incoming request contains an expected `Referer` header value (for example, a request coming from an admin page). By setting the `Referer` header to that expected value, an attacker can bypass this weak protection and trigger admin functionality.

#### ✅ Why it's dangerous

* The `Referer` header is client-supplied and can be forged or omitted; it's insufficient for enforcing access control.
* Attackers can easily simulate legitimate referers using proxies or request tools and invoke privileged endpoints.
* Reliance on Referer leads to trivial authorization bypasses and compromise of protected actions.

#### ✅ Where to look

* Endpoints that perform privileged actions and check `Referer` before proceeding.
* Any server-side logic or middleware that uses `Referer` as a gate for functionality.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker captures the admin action request and notes that the server expects a specific `Referer` header value.
* The attacker replays the request in Burp Repeater or Proxy and sets the `Referer` to the expected admin page URL (or the value observed).
* The server accepts the modified request and processes the admin action (e.g., deleting `carlos`).

**Impact:** Authorization bypass via header spoofing — attackers can perform admin actions without valid privileges, leading to account takeover or destructive actions.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker account and navigated to the site to find potential admin actions to target.
2. Captured the admin action request in Burp Proxy and inspected the `Referer` header value used in legitimate admin requests.
3. Replayed the request in Repeater and replaced the `Referer` header with the expected admin page URL (or the value observed).
4. Sent the modified request and observed the server response indicating the privileged action succeeded (e.g., user deleted).
5. Verified `carlos` was removed from the site to confirm the lab was solved.

---

### 📸 Screenshots
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-13/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-13/images/2.png)

---

### 📝 What I Learned

✔ Do not rely on client-supplied headers like `Referer` for access control decisions.                                        
✔ Header values can be spoofed by attackers using proxies or request tools — always enforce server-side authorization.                              
✔ Test for header-based protections by modifying or removing headers in requests to see how the server responds.                           

---

### 🔐 Mitigation Techniques
               
✔ Implement robust server-side authorization checks that do not depend on `Referer` or other client-controlled headers.                                                  
✔ Use authenticated session checks, role-based access control, and centralized authorization middleware for privileged actions.                                                        
✔ If `Referer` is used for low-confidence checks (e.g., CSRF mitigation), combine with stronger measures such as CSRF tokens and server-side validation.                             

---

### 🧾 Notes

* The lab requires forging the `Referer` header to trigger deletion of the `carlos` account.
* Only perform such testing in authorized lab environments; in production, ensure header-based heuristics are not relied upon for authorization.

---

### 👤 Author

Harbeer-Singh
