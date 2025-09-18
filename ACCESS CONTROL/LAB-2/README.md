# ✅ **PortSwigger Lab: Unprotected admin functionality — unpredictable URL**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Vertical privilege escalation → Unprotected functionality

---

### 📚 **Description**

This lab demonstrates an **unprotected administrative interface** that is located at an unpredictable URL. The application exposes an admin panel at a non-obvious path, but the panel still lacks proper access control. The goal is to find the admin URL (by enumerating or discovering clues), visit it, and delete the user `carlos` using the admin interface.

The lab illustrates that obscurity (an unpredictable URL) is not a substitute for proper authentication and authorization — if the endpoint is reachable and unprotected, an attacker who discovers it can perform privileged actions.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

An admin interface exists at a non-standard, unpredictable path. Although the path is not obvious, it can be discovered through enumeration or by inspecting site resources. Once found, the admin UI is accessible without authentication, allowing privileged actions.

#### ✅ Why it's dangerous

* Unprotected admin functionality at even an obscure URL grants full administrative control to attackers who discover it.
* Relying on obscurity for protection is insufficient; server-side access controls must be enforced for every privileged endpoint.
* Attackers can enumerate directories, check `robots.txt`, or brute-force common admin paths to find such endpoints.

#### ✅ Where to look

* Explore `robots.txt`, sitemaps, hidden directories, guessable admin paths, or any server responses that might leak the path (e.g., debug pages, error messages).
* Use automated discovery tools (directory brute-force) or manual inspection of the site to find the unpredictable admin path.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The lab places an admin panel at an unpredictable URL that is not linked from the main site.
* After discovering the URL, visiting it reveals an admin UI listing users and administrative actions.
* Because the admin panel lacks authentication/authorization checks, an attacker can delete the `carlos` account via the interface.

**Impact:** Full administrative actions are possible without credentials — critical security risk.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Reviewed `robots.txt` and other site resources for hints about hidden paths.
2. Ran a directory-enumeration attempt (or manually inspected common admin path patterns) to discover the unpredictable admin URL.
3. Navigated to the discovered admin URL and inspected the admin interface.
4. Located the user list and used the admin UI to delete `carlos`.
5. Confirmed the deletion via the admin UI or by attempting to view the account.

---

### 📸 Screenshots 
1. **Request**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-2/images/1.png)

2. **Completed**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-2/images/2.png)

---

### 📝 What I Learned

✔ Obscurity of URLs is not a security control — endpoints must enforce authorization.      
✔ Directory enumeration and inspection of public resources can reveal hidden admin endpoints.       
✔ Regularly audit production systems for unprotected privileged endpoints and remove sensitive paths from public exposure.      

---

### 🔐 Mitigation Techniques

✔ Enforce strict server-side authentication and authorization for all admin endpoints.      
✔ Avoid relying on unpredictable URLs as a security measure — treat all endpoints as discoverable.      
✔ Remove sensitive paths from public files like `robots.txt` and ensure they are protected by access controls.      
✔ Monitor access to admin endpoints and alert on unexpected requests.      
 
---

### 🧾 Note

* The lab solution involves discovering the unpredictable admin path and deleting `carlos` via the admin UI.
* Only perform these actions in authorized lab environments; in production, ensure proper access controls are in place.

---

### 👤 Author

Harbeer-Singh

