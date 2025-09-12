

# ✅ PortSwigger Lab: File Path Traversal – Validate Start of Path

**🔗 Lab Link:**  
[https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path](https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path)

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → File Path Traversal

---

## 📚 Description

This lab demonstrates a **File Path Traversal** vulnerability caused by improper validation of the start of a file path. The application transmits the full file path via a request parameter and validates that the supplied path starts with the expected folder. However, it fails to properly handle traversal sequences, allowing attackers to access files outside the intended directory.

The goal is to exploit this flaw and access sensitive files outside the intended directory, such as `/etc/passwd`.

---

## ⚡ Key Takeaways

### ✅ What is File Path Traversal?  
File Path Traversal is a security vulnerability that enables attackers to access files and directories stored outside of the intended directory by manipulating file paths.

### ✅ What is Validation of Start of Path?  
Validation of start of path is a security measure where the application checks if the supplied file path begins with a specific directory. However, improper implementation can allow attackers to bypass this validation and access unauthorized files.

### ✅ Vulnerable Parameter  
The `filename` parameter is processed by the application, and improper validation allows traversal sequences to bypass the intended directory check.

---

## ⚙️ Explanation of Payload Behavior and Impact

- **Payload:** Path traversal sequences (e.g., `/var/www/images/../../../etc/passwd`) that bypass the start-of-path validation.  
- **Behavior:** The application validates that the path starts with `/var/www/images`, but it fails to properly handle traversal sequences, allowing access to `/etc/passwd`.  
- **Impact:** The attacker can access sensitive system files by exploiting the improper validation, resulting in unauthorized file access.

---

## 🧱 Commands Used

```http
GET /image?filename=/var/www/images/../../../../../../../etc/passwd HTTP/2
```

---

## 📸 Screenshots

**Request**  
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-5/images/1.png)

**Completed**  
![Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-5/images/2.png)


---

## 📝 What I Learned

- ✅ The importance of properly validating file paths to prevent unauthorized access.  
- ✅ How attackers can exploit improper validation to access sensitive files.  
- ✅ Practical experience using tools like Burp Suite to craft payloads and bypass input filters.  
- ✅ The critical need for secure coding practices such as thorough input validation.

---

## 🔐 Mitigation Techniques

- ✅ **Strict Input Validation:** Validate and sanitize input to ensure it conforms to expected patterns and does not contain traversal sequences.  
- ✅ **Canonicalization:** Use secure APIs to resolve the absolute path and verify it starts with the intended directory.  
- ✅ **Least Privilege:** Restrict the application's file access to necessary directories and files only.  
- ✅ **Security Headers:** Implement security headers to prevent unauthorized access and mitigate potential attacks.

---

## 👤 Author

Harbeer-Singh
```
