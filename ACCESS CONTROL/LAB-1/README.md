# ✅ **PortSwigger Lab: Unprotected admin functionality**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Vertical privilege escalation → Unprotected functionality

---

### 📚 **Description**

This lab contains an **unprotected admin panel** that is discoverable via information disclosure. The goal is to locate the admin panel and delete the user `carlos` using the admin interface.

The lab demonstrates how hidden endpoints (often listed in `robots.txt` or other site resources) can expose privileged functionality if they are not properly access-controlled.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

An admin-only interface exists on a predictable path but is not protected by authentication or authorization checks. The path is disclosed in `robots.txt`, allowing anyone who finds it to reach the admin functionality.

#### ✅ Why it's dangerous

* Unprotected administrative endpoints allow attackers to perform privileged actions (delete users, modify data, etc.) without credentials.
* Information disclosure through public files like `robots.txt` or sitemaps can reveal sensitive paths that must still be access-controlled.
* Securing endpoints requires both hiding sensitive URLs and, critically, enforcing proper server-side authorization.

#### ✅ Where to look

* `robots.txt`, `sitemap.xml`, publicly accessible directories, and other site resources that may reveal hidden paths.
* Any `/admin*`, `/administrator*`, or similarly named endpoints and their access controls.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The site’s `robots.txt` discloses the path to the administrator panel (e.g., `/administrator-panel`).
* Visiting that path loads an admin interface that lists users and provides administrative actions.
* Because the admin panel lacks proper authentication/authorization, an attacker can delete the `carlos` user via the interface, resulting in account removal and denial of service for that user.

**Impact:** Full administrative actions are possible without authentication — critical severity for production systems.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Visited the lab and opened `/robots.txt` to inspect any disallowed paths.
2. Noted the `Disallow` line that revealed the admin panel path.
3. Replaced `/robots.txt` in the URL bar with the disclosed admin path (e.g., `/administrator-panel`) and navigated to it.
4. Used the admin interface to locate and delete the user `carlos`.
5. Confirmed the user was removed by checking the admin UI or attempting to view the account.

---

### 📸 Screenshots 

1. **Request**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-1/images/1.png)

2. **Completed**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-1/images/2.png)

---

### 📝 What I Learned

✔ Publicly accessible files like `robots.txt` can leak sensitive paths — but secrecy alone is not protection.          
✔ All administrative functionality must enforce server-side authentication and authorization, regardless of whether the path is obscure. 
✔ Regularly audit public site resources for information disclosure and ensure endpoints referenced are protected.  

---

### 🔐 Mitigation Techniques

✔ Implement strict server-side authentication and authorization checks for all admin endpoints.         
✔ Do not rely on obscurity (hidden paths) as a security measure; treat any endpoint as potentially discoverable.        
✔ Avoid listing truly sensitive paths in `robots.txt` or similar public files; instead use robots rules only for benign crawl guidance.        
✔ Monitor access to admin endpoints and alert on unexpected requests.                 

---

### 🧾 Notes

* The lab solution uses `robots.txt` to discover `/administrator-panel` and then deletes the user `carlos`.
* Only perform these actions in authorized lab environments; in production, ensure proper access controls are enforced.

---

### 👤 Author

Harbeer-Singh

