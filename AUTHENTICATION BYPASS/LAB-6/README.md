# ✅ **PortSwigger Lab: Broken brute-force protection — multiple credentials per request**

## 🔗 **Lab Link:**

[https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request](https://portswigger.net/web-security/authentication/password-based/lab-broken-brute-force-protection-multiple-credentials-per-request)

## ⚙️ **Difficulty:** Expert

📂 **Category:** Authentication → Password-based → Brute-force protections

---

### 📚 **Description**

This lab demonstrates a logic flaw in brute-force protection where the application accepts multiple credential values in a single JSON request (an array of passwords) and processes them in a way that allows an attacker to log in as a victim without triggering per-IP or per-account rate-limits.

**Goal:** Brute-force the victim `carlos`'s password by supplying multiple candidate passwords in one request, then access the victim's account page.

---

### ⚡ **Key Takeaways**

#### ✅ What is happening?

The login endpoint accepts JSON and will process an array of passwords submitted in a single request. By sending all candidate passwords together, the server attempts them and returns a successful login when one matches — bypassing naive brute-force protections.

#### ✅ Why it's dangerous

* Allows rapid credential testing without tripping rate limits that monitor per-request or per-IP failure counts.
* Logical handling of array inputs can result in unintended authentication behavior.
* Attackers can log in as victims by abusing input formats the server does not expect.

#### ✅ Where to look

* Endpoints that accept JSON payloads for authentication (e.g., `POST /login`) and how they parse array vs scalar values.
* Any input-parsing logic that treats arrays specially or stops at first success.

---

### ⚙️ **Explanation of Observed Behavior and Impact**

* The server’s login handler iterates over the supplied array of passwords and treats any matching entry as a successful authentication, returning a `302` redirect for success.
* Because several candidate passwords are checked in one request, counters that increment on each failed request are not triggered in the same way, allowing an attacker to test many passwords while avoiding detection.

**Impact:** An attacker can quickly test many credentials and take over accounts without triggering common brute-force protections.

---

### 🧪 Methodology — How I approached it (lab-appropriate, no command snippets)

1. Inspected the login request and observed it accepted JSON-formatted credentials.                                   
2. In Burp Repeater, modified the request body so that the `password` value was an array containing multiple candidate passwords (the lab's candidate password list).                         
3. Sent the modified request and observed a `302` response indicating a successful login when one of the supplied passwords matched.    
4. Opened the response in the browser and navigated to "My account" to confirm access as `carlos` and complete the lab.         

---

### 📸 Screenshots (serially numbered — include these exact filenames in the repo)

1. **Request**
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-6/images/1.png)

2. **Completed**
  ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/AUTHENTICATION%20BYPASS/LAB-6/images/2.png)

---

### 📝 What I Learned

✔ Servers may behave unexpectedly when given arrays where scalars are expected.
✔ Input-parsing logic is a frequent source of subtle authentication bugs.
✔ Brute-force protections must account for alternative request shapes (arrays, JSON, multipart, etc.).
✔ Burp Repeater is useful for experimenting with request modifications to understand server behavior.

---

### 🔐 Mitigation Techniques

✔ Strictly validate and sanitize input types — reject arrays where a single string is expected.                  
✔ Implement robust brute-force protections that count failed authentication attempts per account, per IP, and per session; ensure they cannot be bypassed by combining multiple attempts in one request.                       
✔ Use rate-limiting, account lockout thresholds, and multi-factor authentication.                    
✔ Log and alert on unusual input shapes or unusually large payloads to authentication endpoints.           

---

### 🧾 Notes

* Victim username provided in the lab: `carlos`.
* The lab highlights how a single request containing multiple candidate passwords can return a successful login if any element of the array matches the real password.
* Only perform these tests in authorized, lab environments.

---

### 👤 Author

Harbeer-Singh

