# ✅ PortSwigger Lab: File Path Traversal – Superfluous URL Decode

**🔗 Lab Link:**  
[https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode](https://portswigger.net/web-security/file-path-traversal/lab-superfluous-url-decode)

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → File Path Traversal

---

## 📚 Description

This lab demonstrates a **File Path Traversal** vulnerability caused by superfluous URL decoding. The application attempts to prevent path traversal attacks by sanitizing and filtering out suspicious path traversal sequences from user-supplied input. However, because of redundant decoding steps, an attacker can bypass these protections by double-encoding the path traversal sequences.

The goal is to exploit this flaw and access sensitive files outside the intended directory, such as `/etc/passwd`.

---

## ⚡ Key Takeaways

### ✅ What is File Path Traversal?  
File Path Traversal is a security vulnerability that enables attackers to access files and directories stored outside of the intended directory by manipulating file paths.

### ✅ What is Superfluous URL Decoding?  
When an application decodes user input more than once, attackers can craft payloads that appear sanitized after one decoding step but revert to malicious content after additional decoding. This can be exploited to bypass input validation.

### ✅ Vulnerable Parameter  
The `filename` parameter is processed by multiple decoding steps, allowing double-encoded traversal sequences to bypass filtering.

---

## ⚙️ Explanation of Payload Behavior and Impact

- **Payload:** Double-encoded path traversal sequences (e.g., `%252e%252e/` interpreted as `../` after two decoding steps).  
- **Behavior:** The application sanitizes input after the first decoding but processes a second decoding later, converting benign-looking input into malicious traversal sequences.  
- **Impact:** The attacker can access sensitive system files by exploiting redundant decoding processes, resulting in unauthorized file access.

---

## 🧱 Commands Used

```http
<!-- Add the command you used here -->
```

---

## 📸 Screenshots

**Request**  
![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/PATH%20TRAVERSAL/LAB-4/images/1.png)

**Completed**  
![Response](https://github.com/Harbeer-Singh/Portswigger-Labs/edit/main/PATH%20TRAVERSAL/LAB-4/images/12.png)



---

## 📝 What I Learned

- ✅ The importance of understanding how multiple decoding steps can unintentionally weaken security controls.  
- ✅ How attackers can exploit redundant or improper input handling in web applications.  
- ✅ Practical experience using tools like Burp Suite to craft double-encoded payloads and bypass input filters.  
- ✅ The critical need for secure coding practices such as thorough input validation and avoiding unnecessary processing steps.

---

## 🔐 Mitigation Techniques

- ✅ **Strict Input Validation:** Validate and sanitize input at every decoding step to prevent traversal attacks.  
- ✅ **Avoid Multiple Decodings:** Ensure input is decoded only once or carefully track and manage decoding processes.  
- ✅ **Use Secure APIs:** Avoid constructing file paths from user input directly; instead, use safe functions that prevent directory traversal.  
- ✅ **Limit File Access:** Restrict the application's file access to necessary directories and files only.

---

## 👤 Author

Harbeer-Singh
```
