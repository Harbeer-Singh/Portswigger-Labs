# 🧪 PortSwigger Lab: SSRF with Whitelist-Based Input Filter

## 🎯 Lab Overview

In this expert-level lab, I explored a Server-Side Request Forgery (SSRF) vulnerability within a product stock checker application. The objective was to manipulate the application into making internal requests to an admin interface, allowing us to delete a specific user. The challenge was heightened by the implementation of a whitelist-based input filter designed to prevent such attacks.

## 🔍 Vulnerability Summary

Server-Side Request Forgery (SSRF) is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location. In this lab, the application fetches data from an internal system based on a user-supplied URL in the `stockApi` parameter. The developer has implemented a whitelist-based input filter to restrict access to external resources. However, this filter can be bypassed using various techniques.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Visit a product and click "Check stock".
- **Tool**: Use Burp Suite to intercept the HTTP request.
- **Details**: The relevant request is a `POST` to `/product/stock` with parameters `productId` and `storeId`.

### 2. Identifying the Whitelist Filter

- **Action**: Modify the `stockApi` parameter to:
     http://127.0.0.1/
- **Observation**: The request is blocked, indicating that the application has implemented a whitelist filter to restrict access to external resources.


### 3. Accessing the Admin Interface

- **Action**: Modify the `stockApi` parameter to:
 http://192.168.0.12:8080/admin/delete?username=carlos
- **Observation**: The user `carlos` is deleted, solving the lab.

## 🧠 Key Takeaways

- **Understanding SSRF**: SSRF vulnerabilities occur when an application fetches data from a user-supplied URL. By manipulating this URL, attackers can direct the application to make requests to internal resources.
- **Bypassing Whitelist Filters**: Whitelist-based input filters can often be bypassed using techniques such as embedding credentials, exploiting URL fragment identifiers, and double URL encoding.
- **Importance of Proper Input Validation**: Proper validation and sanitization of user inputs are crucial to prevent SSRF vulnerabilities.

## ⚠️ Challenges Faced

- **Identifying the Vulnerable Parameter**: Determining which parameter was used to fetch data from the internal system required careful analysis of the application's behavior.
- **Bypassing Filters**: Accessing the admin interface was blocked by filters, necessitating the use of encoding techniques to bypass these controls.

## 💡 Tips for Others

- **Thorough Testing**: Test all user inputs, including hidden fields and HTTP headers, for potential SSRF vulnerabilities.
- **Use of Burp Suite**: Leverage Burp Suite's Proxy and Repeater tools to intercept, modify, and resend requests efficiently.
- **Learning Resources**: Refer to PortSwigger's [SSRF documentation](https://portswigger.net/web-security/ssrf) for in-depth understanding and examples.

## 📸 Screenshots

1. **Request**  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SSRF/LAB-5/images/1.png)

2. **Output**  
 ![Modified Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SSRF/LAB-5/images/2.png)
