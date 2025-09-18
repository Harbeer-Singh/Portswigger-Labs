# ✅ **PortSwigger Lab: User role can be modified in user profile**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile](https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Vertical privilege escalation → Authorization logic

---

### 📚 **Description**

This lab demonstrates an **authorization vulnerability** where a user can elevate their own privileges by editing a role-related field in their profile. The application exposes a form or API that includes a `role` (or equivalent) attribute which the server accepts from the client and uses to determine privileges. By changing this value to `admin`, the attacker can access admin-only functionality such as deleting other users.

**Goal:** Modify your profile to set an administrative role, then use the elevated privileges to delete the user `carlos`.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The server trusts a client-supplied role value when processing profile updates. This allows attackers to escalate privileges simply by changing a form parameter or JSON property in their profile update request.

#### ✅ Why it's dangerous

* Client-controllable role data completely breaks access control boundaries.
* Attackers can perform privileged actions (delete users, change data) without needing credentials for an admin account.
* Hidden or poorly-validated profile fields are often overlooked and can lead to severe privilege escalation.

#### ✅ Where to look

* Profile edit forms and corresponding endpoints (`/my-account`, `/profile`, etc.) that accept role, isAdmin, or similar attributes.
* JSON APIs and hidden input fields — both can be tampered with to alter role values.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The profile update request includes a `role` field that the server persistingly uses to determine the account’s privileges.
* Editing that field client-side to `admin` (via Burp Repeater/Proxy or by modifying the form) results in the server granting administrative privileges to the attacker account.
* With admin privileges, the attacker can access admin actions such as deleting `carlos` and confirming the deletion via the admin UI.

**Impact:** Immediate privilege escalation and administrative control through simple client-side tampering — critical for production systems.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in with the attacker account provided by the lab and navigated to the profile edit page.
2. Captured the profile update request in Burp Proxy and examined the parameters or JSON body.
3. Located the `role` (or equivalent) attribute and modified its value to `admin` in Burp Repeater/Proxy.
4. Sent the modified request and then navigated to the admin interface or attempted an admin-only action (delete user `carlos`).
5. Confirmed the action succeeded and the account `carlos` was removed.

---

### 📸 Screenshots
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-4/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-4/images/2.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-4/images/3.png)

---

### 📝 What I Learned

✔ Never trust client-supplied role information — determine roles server-side from authenticated session state.                   
✔ Hidden fields in forms and JSON properties are easy to tamper with; enforce server-side authorization checks for every privileged action.                           
✔ Regularly review profile and account-management endpoints for unexpected writable attributes.                      

---

### 🔐 Mitigation Techniques

✔ Ignore client-supplied role identifiers on the server; derive user roles from authenticated server-side session data.                         
✔ Validate and sanitize all profile update inputs and enforce authorization checks on privileged operations.                         
✔ Implement centralized role-based access control (RBAC) and minimize client-controlled metadata that affects privileges.                    

---

### 🧾 Notes

* The lab provides an attacker account and the victim username `carlos`.
* This vulnerability is a classic example of trusting client input for authorization decisions — always validate server-side.
* Only perform such testing in authorized lab environments.

---

### 👤 Author

Harbeer-Singh
