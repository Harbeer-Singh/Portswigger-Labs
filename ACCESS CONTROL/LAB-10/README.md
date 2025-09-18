# ✅ **PortSwigger Lab: URL-based access control can be circumvented**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented](https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → URL-based access control → Authorization bypass

---

### 📚 **Description**

This lab demonstrates how **URL-based access control**—where the application relies on the request path to determine permissions—can be bypassed. The application exposes admin-only functionality under specific paths, but authorization checks are performed by hiding or placing links behind UI elements rather than enforcing server-side controls. The goal is to access the admin functionality and delete the user `carlos` by discovering and hitting the unprotected endpoint.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application assumes obscuring admin URLs or controlling access via UI navigation is sufficient. However, the server does not enforce authorization for the endpoint itself, allowing attackers to access admin actions by requesting the URL directly.

#### ✅ Why it's dangerous

* Relying on URL structure or frontend controls (e.g., hiding links) without server-side authorization checks is insecure.
* Attackers can craft requests to privileged endpoints directly and perform admin actions if the server trusts the URL/path to determine access.
* This leads to privilege escalation and exposure of administrative functionality to unauthenticated or low-privilege users.

#### ✅ Where to look

* Admin or privileged endpoints that appear protected only by being unlinked or behind client-side checks.
* Server responses for direct requests to administrative paths and whether they enforce authentication/authorization.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The admin interface is reachable at a specific path (e.g., `/admin` or similar) that the site does not link to for normal users.
* Requesting that path directly in the browser or via a proxy returns the admin UI or an endpoint that performs admin actions, because the server fails to check the requester’s privileges.
* Using that interface, an attacker can delete the user `carlos` and complete the lab.

**Impact:** Complete administrative actions without the proper privileges — critical security issue for production systems.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Inspected the site for hidden or admin-related paths (`/admin`, `/administrator`, `/manage`, etc.) and checked `robots.txt` and other public resources for hints.
2. Manually requested likely admin paths or used a directory enumerator until an admin endpoint responded.
3. Navigated to the admin UI and used the available controls to delete the user `carlos`.
4. Verified the deletion via the admin UI or by attempting to access the account.

---

### 📸 Screenshots 

![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-11/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-11/images/2.png)

---

### 📝 What I Learned

✔ URL obscurity is not access control — enforce server-side authorization checks for all privileged endpoints.                            
✔ Regularly audit server responses to direct requests for admin paths and ensure they require appropriate authentication and authorization.                         
✔ Use centralized authorization middleware to prevent bypasses caused by relying on URL patterns or frontend restrictions.                             

---

### 🔐 Mitigation Techniques

✔ Enforce strict server-side authorization for every privileged endpoint regardless of how it is linked in the UI.                             
✔ Avoid relying on client-side checks or hidden links as protection; implement role-based access control on the server.                                     
✔ Log and monitor direct access attempts to admin paths and require multi-factor authentication for admin operations.                                  

---

### 🧾 Notes

* The lab solution involves discovering the unprotected admin endpoint and deleting `carlos` via the admin UI.
* Only perform such actions in authorized lab environments; in production, ensure admin endpoints are properly protected.

---

### 👤 Author

Harbeer-Singh
