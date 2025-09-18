# ✅ **PortSwigger Lab: Insecure Direct Object References (IDOR)**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Insecure Direct Object References (IDOR) / Horizontal privilege escalation

---

### 📚 **Description**

This lab demonstrates an **Insecure Direct Object Reference (IDOR)** where an application exposes a parameter that directly references user-owned resources (for example, an `id` parameter). The server fails to enforce object-level authorization and returns another user's data when the parameter is tampered with. The goal is to exploit this flaw to access and delete the user `carlos`'s account data.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application uses predictable identifiers in requests and does not verify that the authenticated user is authorised to access the referenced resource. By changing the identifier parameter in a request, an attacker can access or manipulate other users’ data.

#### ✅ Why it's dangerous

* IDORs allow horizontal privilege escalation and direct access to sensitive data belonging to other users.
* Attackers can exploit such flaws to read, modify, or delete records without authentication as the target user.
* These issues are common when server-side authorization checks are missing or insufficient.

#### ✅ Where to look

* Any endpoint that takes an object identifier (e.g., `id`, `user`, `account`), especially profile pages, file downloads, or account management APIs.
* Hidden or predictable numeric IDs, GUIDs, or filenames that directly map to sensitive resources.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker captures a request for their own account/resource and identifies the parameter that controls which account is returned.
* Modifying that parameter to reference `carlos` (or his ID) causes the server to return Carlos’s account page or allow actions on his account, because it does not check whether the requester is authorised for that object.
* Using the exposed functionality, the attacker can delete Carlos’s account and confirm the action to solve the lab.

**Impact:** Confidentiality and integrity violations — unauthorized access and destructive actions against other users’ accounts.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker account and navigated to the "My account" page to capture the request in Burp Proxy.
2. Identified the object identifier parameter (e.g., `id`) in the request URL or body.
3. Modified the identifier value to point to the victim `carlos` and resent the request using Burp Repeater or by editing the URL in the browser.
4. Confirmed the response contained Carlos’s account details or actions.
5. Used the available functionality to delete Carlos’s account and verified the deletion in the admin or user view.

---

### 📸 Screenshots 

![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-9/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-9/images/2.png)

---

### 📝 What I Learned

✔ IDORs are a frequent source of privilege escalation — always check authorization for object-level access.                               
✔ Predictable identifiers make exploitation easier; consider using indirect references or verifying ownership server-side.                         
✔ Parameter tampering is a simple but powerful testing technique for discovering access control flaws.                                  

---

### 🔐 Mitigation Techniques

✔ Enforce server-side object-level authorization checks for every request that references user-specific resources.                        
✔ Use non-guessable indirect references where practical and validate ownership before returning sensitive data.                                
✔ Perform regular authorization reviews and include negative testing for parameter tampering in QA and security testing.                    

---

### 🧾 Notes

* The lab provides attacker credentials and requires deleting the victim `carlos`.
* Only perform these tests in authorized lab environments.

---

### 👤 Author

Harbeer-Singh


