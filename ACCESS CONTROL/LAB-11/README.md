# ✅ **PortSwigger Lab: Method-based access control can be circumvented**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented](https://portswigger.net/web-security/access-control/lab-method-based-access-control-can-be-circumvented)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Method-based access control → Authorization bypass

---

### 📚 **Description**

This lab demonstrates how **method-based access control** (authorisation enforced by HTTP method checks) can be bypassed if the server relies solely on the request method (GET/POST/PUT/DELETE) to determine whether an action is allowed. The goal is to perform an administrative action (delete user `carlos`) by crafting a request that the server incorrectly treats as an allowed method for performing the action.

Method-based controls are fragile when the server assumes that only certain HTTP methods will reach sensitive endpoints. Attackers can manipulate requests (method tunnelling via headers, request smuggling-like variations, or sending parameters that trigger the same server-side handler) to bypass these checks.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application assumes that administrative actions are only reachable via certain HTTP methods (e.g., `POST` or `DELETE`) and trusts the client to use the correct method. By sending the request using an allowed method or by using alternative request shapes that the server accepts (for example, using `POST` with a `_method=DELETE` parameter or by toggling `Content-Type`), the attacker can trigger admin functionality without proper authorization.

#### ✅ Why it's dangerous

* Relying on HTTP methods alone for authorization is insufficient — method checks must be combined with authentication/authorization controls.
* Attackers can craft requests to reach server-side handlers that perform privileged actions, even if the UI doesn't expose those methods.
* This allows unauthorized users to perform admin actions such as deleting accounts.

#### ✅ Where to look

* Endpoints that perform state-changing actions and that appear to rely on the request method for access control.
* Hidden form parameters (e.g., `_method`) or alternate content-types that may be accepted by server frameworks.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker inspects the application’s admin action flow and captures the request that performs the delete action (often a `POST` or `DELETE` to a particular path).
* By re-sending the request with a different method or by adding a method-override parameter (e.g., `_method=DELETE`) the server processes the request and executes the privileged action even though the attacker is not authorized.
* Using this technique, the attacker deletes the `carlos` account and completes the lab.

**Impact:** Unauthorized modification or deletion of data via method-based control bypass — critical privilege escalation risk.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker account and browsed the application to find candidate endpoints that perform state changes (user deletion).
2. Captured the request that performs the delete action (note the method and parameters).
3. Replayed the request in Burp Repeater and attempted alternative methods or request shapes: for example, using `POST` with a `_method=DELETE` parameter, changing `Content-Type`, or sending the same parameters to a different path the server accepts.
4. Observed a server response indicating the action succeeded (e.g., a 200/302 or JSON success).
5. Confirmed `carlos` was deleted by reloading the relevant page or checking the user list.

---

### 📸 Screenshots
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-11/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-11/images/2.png)

---

### 📝 What I Learned

✔ Method-based checks are not a substitute for proper authentication and authorization.                                        
✔ Many frameworks support method-override techniques (hidden `_method` fields or special headers) — these must be accounted for in access control design.                                              
✔ During testing, try alternate methods and method-override patterns to discover hidden handlers.                         
               
---

### 🔐 Mitigation Techniques

✔ Enforce server-side authorization checks independent of HTTP method — verify the authenticated user’s privileges for every state-changing action.                  
✔ Disable or validate method-override features if not needed, and sanitize accepted content-types and parameters.                         
✔ Use centralized middleware to check permissions and log attempts to perform privileged actions via non-standard methods.                        

---

### 🧾 Notes

* The lab requires exploiting a method-based access control weakness to delete the `carlos` account.
* Only perform such testing in authorized lab environments; in production, ensure method-based handlers are protected properly.

---

### 👤 Author

Harbeer-Singh

