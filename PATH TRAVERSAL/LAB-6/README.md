# ✅ PortSwigger Lab: File Path Traversal – Validate File Extension Null Byte Bypass

**🔗 Lab Link:**  
[https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass](https://portswigger.net/web-security/file-path-traversal/lab-validate-file-extension-null-byte-bypass)

**⚙️ Difficulty:** PRACTITIONER  
**📂 Category:** Web Security → File Path Traversal

---

## 📚 Description

This lab demonstrates a **File Path Traversal** vulnerability caused by improper validation of file extensions. The application validates that the supplied filename ends with the expected file extension. However, it fails to properly handle null byte characters, allowing attackers to bypass the extension check and access files outside the intended directory.

The goal is to exploit this flaw and access sensitive files outside the intended directory, such as `/etc/passwd`.

---

## ⚡ Key Takeaways

### ✅ What is File Path Traversal?  
- File Path Traversal is a security vulnerability that enables attackers to access files and directories stored outside of the intended directory by manipulating file paths.

### ✅ What is Null Byte Injection?  
- Null Byte Injection is a technique where an attacker appends a null byte (`%00`) to the end of a filename to terminate the string early, bypassing extension checks and other validations.

### ✅ Vulnerable Parameter  
- The `filename` parameter is processed by the application, and improper validation allows null byte injection to bypass the file extension check.

---

## ⚙️ Explanation of Payload Behavior and Impact

- **Payload:**  
  Path traversal sequence followed by a null byte and the expected file extension (e.g., `../../../etc/passwd%00.png`).

- **Behavior:**  
  The application validates that the path ends with `.png`, but the null byte terminates the string early, allowing access to `/etc/passwd`.

- **Impact:**  
  The attacker can access sensitive system files by exploiting the improper validation, resulting in unauthorized file access.

---

## 🧱 Commands Used

```http
GET /image?filename=../../../../../../../etc/passwd%00.png HTTP/2
```

---

## 📸 Screenshots

**Request**  
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-6/images/1.png)

**Completed**  
![Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-6/images/2.png)

---

## 📝 What I Learned

- ✅ The importance of properly validating file extensions to prevent unauthorized access.  
- ✅ How attackers can exploit null byte injection to bypass security checks.  
- ✅ Practical experience using tools like Burp Suite to craft payloads and bypass input filters.  
- ✅ The critical need for secure coding practices such as thorough input validation.

---

## 🔐 Mitigation Techniques

- ✅ **Strict Input Validation:**  
  Validate and sanitize input to ensure it conforms to expected patterns and does not contain traversal sequences or null bytes.

- ✅ **Canonicalization:**  
  Use secure APIs to resolve the absolute path and verify it starts with the intended directory.

- ✅ **Least Privilege:**  
  Restrict the application's file access to necessary directories and files only.

- ✅ **Security Headers:**  
  Implement security headers to prevent unauthorized access and mitigate potential attacks.

---

## 👤 Author

Harbeer-Singh
