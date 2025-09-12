# ✅ PortSwigger Lab: File Path Traversal – Sequences Stripped Non-Recursively

**🔗 Lab Link:**  
[https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → File Path Traversal

---

## 📚 Description

This lab demonstrates a **File Path Traversal** vulnerability in a web application’s image filename parameter. The application attempts to mitigate path traversal attacks by stripping path traversal sequences from the user-supplied filename before using it. However, this stripping is done only once and does not account for multiple occurrences within the input. By exploiting this limitation, an attacker can bypass the restriction and access files outside the intended directory.

The objective is to retrieve the contents of the `/etc/passwd` file, which contains user account information on Unix-based systems.

---

## ⚡ Key Takeaways

### ✅ What is File Path Traversal?  
File Path Traversal is a vulnerability that allows attackers to access files and directories outside the intended directory by manipulating file paths. This can lead to unauthorized access to sensitive files on the server.

### ✅ Vulnerable Parameter  
The `filename` parameter is used to specify the image file to be displayed. The application strips path traversal sequences from the user-supplied filename before using it. However, this stripping is done only once and does not account for multiple occurrences within the input.

---

## ⚙️ Explanation of Payload Behavior and Impact

- **Payload:** `....//....//....//etc/passwd`  
- **Behavior:** The application strips the first occurrence of the path traversal sequence (`....//`) from the filename. However, it does not strip subsequent occurrences, allowing the attacker to effectively traverse directories multiple times.  
- **Impact:** This allows the attacker to access sensitive system files, such as `/etc/passwd`, leading to potential information disclosure.

---

## 🧱 Commands Used

```http
GET /image?filename=....//....//....//etc/passwd HTTP/2
```

---

###📸 Screenshots
1. **Request**  
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-4/images/1.png)

2. **Completed**  
   ![Time Delay Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/PATH%20TRAVERSAL/LAB-4/images/2.png)


Replace the placeholder filenames with your actual screenshots.

---

###📝 What I Learned
✔ The importance of properly sanitizing user inputs to prevent path traversal vulnerabilities.                                 
✔ How incomplete sanitization can lead to security vulnerabilities.                             
✔ Practical experience with using Burp Suite to intercept and modify HTTP requests to exploit vulnerabilities.                       
✔ The potential risks associated with information disclosure through improper handling of file paths.                       

---

###🔐 Mitigation Techniques
✔ Input Validation: Ensure that user-supplied file paths are validated against a whitelist of allowed files or directories.                                
✔ Sanitization: Remove or encode special characters that could be used for path traversal.                            
✔ Use of Safe APIs: Utilize secure functions that do not allow direct manipulation of file paths.                                        
✔ Least Privilege: Run applications with the minimum necessary permissions to limit access to sensitive files.                                    

---

###👤 Author
Harbeer-Singh
