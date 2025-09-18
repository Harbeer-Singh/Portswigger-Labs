# ✅ **PortSwigger Lab: User ID controlled by request parameter**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter)

## ⚙️ **Difficulty:** Apprentice

📂 **Category:** Access control → Horizontal privilege escalation → Insecure Direct Object References (IDOR)

---

### 📚 **Description**

This lab demonstrates an **Insecure Direct Object Reference (IDOR)** vulnerability where user-specific data is accessed using a `userId` parameter in the request. The application does not enforce proper access controls, allowing attackers to modify the parameter and access or perform actions on other users' accounts.

**Goal:** Exploit the IDOR vulnerability to access the account page of the victim user `carlos` and delete their account.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The application uses a request parameter (e.g., `id`, `user`, or `userid`) to determine which account data to display. Instead of verifying whether the authenticated user is authorized to view or modify the specified account, the server simply trusts the parameter’s value.

#### ✅ Why it's dangerous

* Attackers can manipulate the `userId` value to access or alter data of other accounts.
* This class of vulnerability enables **horizontal privilege escalation** and can lead to exposure of sensitive information or account takeover.
* Such flaws are often widespread in applications that rely on predictable identifiers and insufficient server-side checks.

#### ✅ Where to look

* Any request parameter (`id`, `userId`, `account`, etc.) that references a resource owned by a user.
* Profile pages, account settings, and API calls returning user-specific data.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The attacker logs in with their own account and notices a `userId` parameter in requests when accessing their account page.
* Changing the `userId` value to `carlos` (or Carlos’s numeric identifier) reveals Carlos’s account page.
* The attacker can then use the exposed delete functionality to remove Carlos’s account.

**Impact:** Unauthorized access and manipulation of other users’ accounts — critical breach of confidentiality and integrity.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Logged in as the attacker user and navigated to their account page.
2. Captured the account page request in Burp Proxy.
3. Identified the `userId` parameter in the request.
4. Modified the value of `userId` from the attacker’s identifier to `carlos`.
5. Resent the modified request and confirmed access to Carlos’s account page.
6. Located the delete functionality in Carlos’s account and performed the deletion.
7. Verified that the account `carlos` was removed.

---

### 📸 Screenshots 
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-6/images/1.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-6/images/2.png)
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/ACCESS%20CONTROL/LAB-6/images/3.png)

---

### 📝 What I Learned

✔ IDOR vulnerabilities are simple yet powerful — they often arise when developers assume identifiers are unguessable.                              
✔ Every request must include proper authorization checks, not just authentication.                       
✔ Parameter tampering during testing can quickly reveal horizontal privilege escalation flaws.                      

---

### 🔐 Mitigation Techniques

✔ Implement robust server-side authorization checks for every request to user-specific resources.                          
✔ Use indirect references (e.g., random tokens) instead of sequential or predictable user IDs.                            
✔ Apply object-level access controls systematically across the application.                                      
✔ Conduct regular penetration testing to uncover IDORs in APIs and web apps.                 
 
---

### 🧾 Notes

* The lab provides attacker credentials and requires deleting the victim `carlos`.
* IDOR flaws are among the most common access control issues and have high real-world impact.

---

### 👤 Author

Harbeer-Singh
