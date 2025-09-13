# ✅ **PortSwigger Lab: File Path Traversal – Simple Case**


## 🔗 **Lab Link:**  
[https://portswigger.net/web-security/file-path-traversal/lab-simple](https://portswigger.net/web-security/file-path-traversal/lab-simple)

## ⚙️ **Difficulty:** Easy  
📂 **Category:** Web Security → File Path Traversal

---

### 📚 **Description**

This lab demonstrates a **File Path Traversal** vulnerability in a web application’s image filename parameter.  
The goal was to retrieve sensitive system files by manipulating the file path, bypassing restrictions on accessible directories.  
It emphasizes how insufficient input sanitization can lead to unauthorized access to server files, and how attackers can exploit directory traversal sequences like `../` to move outside the intended directory structure.

---

### ⚡ **Key Takeaways**

#### ✅ What is File Path Traversal?  
File Path Traversal is a vulnerability where attackers manipulate file paths by injecting traversal sequences (e.g., `../`) to access files and directories outside the authorized directory.

#### ✅ Vulnerable Parameter  
The `filename` parameter was used directly by the server to access files, without proper validation or restriction.

---

### ⚙️ **Explanation of Payload Behavior and Impact**

- By using a payload like `../../../etc/passwd`, the attacker traversed up the directory hierarchy to access sensitive system files.
- The server improperly trusted user input and exposed file contents, which could lead to leakage of configuration files, credentials, or other sensitive data.

---

### 🧱 **Commands Used**

```http
GET /image?filename=../../../etc/passwd HTTP/2
```
📸 Screenshots

1. **Request**  
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-1/images/1.png)

2. **Completed**  
   ![Time Delay Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-1/images/2.png)
   

### 📝 What I Learned

✔ The critical importance of restricting file access based on user input.               
✔ How directory traversal sequences (../) can bypass directory protections.                    
✔ The need for input sanitization and strict path validation in file handling functions.             
✔ Practical use of intercepting requests to modify parameters and test vulnerabilities.                      

### 🔐 Mitigation Techniques

✔ Validate and sanitize user-supplied input thoroughly before using it to access files.             
✔ Use fixed directories or map file paths internally rather than trusting user-provided paths.                 
✔ Implement strict checks to prevent traversal sequences in file paths.                             
✔ Deploy a Web Application Firewall (WAF) to block common path traversal patterns.                  

---
                  
### 👤 Author
Harbeer-Singh
