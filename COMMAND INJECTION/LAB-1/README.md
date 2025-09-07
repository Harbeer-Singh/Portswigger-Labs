# 🧪 PortSwigger Lab: OS Command Injection – Simple Case

## 🎯 Lab Overview

In this lab, I explored a fundamental OS Command Injection vulnerability within a product stock checker application. The goal was to execute the `whoami` command on the server to determine the current user, thereby demonstrating the exploitation of insufficient input sanitization in web applications.

## 🔍 Vulnerability Summary

OS Command Injection, also known as shell injection, occurs when user input is improperly passed to a system shell. This allows attackers to execute arbitrary OS commands on the server, potentially compromising the application and its data. In this case, the application executed a shell command containing user-supplied product and store IDs, returning the raw output in its response.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

Using Burp Suite's Proxy feature, I intercepted the HTTP request made when checking the product stock. The relevant request was a `POST` to `/product/stock` with parameters `productId=1` and `storeId=London`.

### 2. Identifying the Vulnerable Parameter

I identified that the `storeId` parameter was being passed directly to a shell command without proper sanitization. This presented an opportunity for OS Command Injection.

### 3. Crafting the Payload

To exploit the vulnerability, I modified the `storeId` parameter to include the `whoami` command:

This payload uses the pipe (`|`) character to chain the `whoami` command to the original command.

## 🧠 Key Takeaways

- **Input Validation**:  
  Always validate and sanitize user inputs before passing them to system commands.

- **Use of Safe APIs**:  
  Prefer using APIs that do not invoke the system shell, such as parameterized database queries or language-specific libraries.

- **Security Testing**:  
  Regularly test applications for OS Command Injection vulnerabilities using tools like Burp Suite.

## ⚠️ Challenges Faced

One challenge was identifying the exact parameter vulnerable to injection. Thorough analysis and testing were required to pinpoint the `storeId` parameter.

## 💡 Tips for Others

- **Thorough Testing**:  
  Test all user inputs, including hidden fields and HTTP headers, for potential injection points.

- **Use of Burp Suite**:  
  Leverage Burp Suite's Proxy and Repeater tools to intercept, modify, and resend requests efficiently.

- **Learning Resources**:  
  Refer to PortSwigger's [OS Command Injection documentation](https://portswigger.net/web-security/os-command-injection) for in-depth understanding and examples.

## Command
**productId=1 %26+whoami+%26+&storeId=1**

## 📸 Screenshots

1. **Intercepted Request**  
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/COMMAND%20INJECTION/LAB-1/images/1.png)

2. **Modified Request**  
   ![Modified Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/COMMAND%20INJECTION/LAB-1/images/2.png)

