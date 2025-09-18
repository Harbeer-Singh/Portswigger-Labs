
# ✅ **PortSwigger Lab: User role controlled by request parameter**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Horizontal/vertical privilege escalation → Authorization logic

---

### 📚 **Description**

This lab demonstrates an **authorization flaw** where the user role is controllable via a client-supplied request parameter. The application trusts a `role` (or similar) parameter sent in requests to determine privileges; by modifying this parameter to `admin` the attacker can access admin-only functionality (for example, deleting users). The goal is to escalate privileges and delete the user `carlos` using the unauthorized admin actions.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application relies on a request parameter to decide the user's role instead of enforcing server-side authorization tied to the authenticated account. This allows an attacker to tamper with the parameter and gain elevated privileges.

#### ✅ Why it's dangerous

* Client-controllable authorization data enables trivial privilege escalation (horizontal or vertical).
* Relying on client input for access control completely breaks the security boundary between users and privileged roles.
* Attackers can perform administrative actions (delete users, modify data) without valid credentials.

#### ✅ Where to look

* Requests that include `role`, `isAdmin`, or similar parameters (hidden form fields, JSON properties, query parameters, or cookies).
* Server-side enforcement points — whether the server actually validates the logged-in user's privileges or simply trusts the supplied value.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The application exposes an endpoint or form that includes a `role` parameter representing the current user's role.
* Editing the parameter value to `admin` (via Burp Repeater/Proxy or by changing the form post) causes the server to treat the request as coming from an admin user.
* Using the elevated role, the attacker can access admin functionality such as deleting `carlos`.

**Impact:** Complete privilege escalation and administrative control through client-side parameter manipulation — critical security failure.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker account (provided by the lab) and browsed the site to find functionality that included role-related parameters (hidden fields, JSON payloads, or query params).
2. Captured a request that contained the role parameter (or a similar control) in Burp Proxy.
3. Replayed the request in Burp Repeater and modified the `role` value to `admin` (or the equivalent elevated value).
4. Sent the modified request and observed that admin-only functionality became available (for example, an admin panel or delete-user endpoint).
5. Used the admin functionality to delete the user `carlos` and confirmed the deletion via the UI or by attempting to view the account.

---

### 📸 Screenshots 
1. **Request**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-3/images/1.png)

2. **Completed**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-3/images/2.png)

---

### 📝 What I Learned

✔ Never trust client-supplied parameters for authorization decisions — always enforce roles server-side based on authenticated user state.                   
✔ Hidden form fields and JSON properties can be tampered with; server-side checks are mandatory.               
✔ Regularly audit endpoints for authorization logic flaws and perform negative testing (tampering with parameters) during reviews.               
                 
---

### 🔐 Mitigation Techniques

✔ Determine user roles on the server using authenticated session data and do not accept client-supplied role identifiers.                     
✔ Remove or ignore role-related hidden fields on the server and validate that the authenticated user is authorized to perform the requested action.                 
✔ Implement centralized authorization checks (policy enforcement) and role-based access control on server-side resources.                
✔ Log attempts to perform privileged actions and monitor for parameter tampering patterns.                

---

### 🧾 Notes

* The lab provides attacker credentials and the target username `carlos`.
* This class of bug is common when developers assume client inputs are trustworthy for role decisions — always validate server-side.
* Only perform such testing in authorized lab environments.

---

### 👤 Author

Harbeer-Singh
