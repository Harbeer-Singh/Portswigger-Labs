# ✅ PortSwigger Lab: File Path Traversal – Absolute Path Bypass

**🔗 Lab Link:**  
[https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → File Path Traversal

---

## 📚 Description

This lab demonstrates a **File Path Traversal** vulnerability in a web application’s image filename parameter. The application attempts to block traversal sequences but treats the supplied filename as relative to a default working directory. By providing an absolute file path, an attacker can bypass these restrictions and access files outside the intended directory.

The objective is to retrieve the contents of the `/etc/passwd` file, which contains user account information on Unix-based systems.

---

## ⚡ Key Takeaways

### ✅ What is File Path Traversal?  
File Path Traversal is a vulnerability that allows attackers to access files and directories outside the intended directory by manipulating file paths. This can lead to unauthorized access to sensitive files on the server.

### ✅ Vulnerable Parameter  
The `filename` parameter is used to specify the image file to be displayed. However, the application does not properly validate or sanitize this input, allowing for manipulation.

---

## ⚙️ Explanation of Payload Behavior and Impact

- **Payload:** `/etc/passwd`  
- **Behavior:** The application treats the provided filename as relative to a default working directory. By supplying an absolute path, the traversal sequence is effectively bypassed.  
- **Impact:** This allows the attacker to access sensitive system files, such as `/etc/passwd`, leading to potential information disclosure.

---

## 🧱 Commands Used

<!-- Add the command you used here -->

---

## 📸 Screenshots

**Screenshot 1 – Intercepted Request in Burp Suite**  
![Screenshot 1](./screenshot1.png)

**Screenshot 2 – Response Containing `/etc/passwd` Contents**  
![Screenshot 2](./screenshot2.png)

> *Replace the placeholder filenames with your actual screenshots.*

---

## 📝 What I Learned

✔ The importance of validating and sanitizing user inputs, especially when they are used to access files or directories.  
✔ How absolute paths can bypass certain security measures intended to prevent path traversal.  
✔ Practical experience with using Burp Suite to intercept and modify HTTP requests to exploit vulnerabilities.  
✔ The potential risks associated with information disclosure through improper handling of file paths.

---

## 🔐 Mitigation Techniques

✔ **Input Validation:** Ensure that user-supplied file paths are validated against a whitelist of allowed files or directories.  
✔ **Sanitization:** Remove or encode special characters that could be used for path traversal.  
✔ **Use of Safe APIs:** Utilize secure functions that do not allow direct manipulation of file paths.  
✔ **Least Privilege:** Run applications with the minimum necessary permissions to limit access to sensitive files.

---

## 👤 Author  
Harbeer-Singh
