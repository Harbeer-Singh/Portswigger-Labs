# 🧪 PortSwigger Lab: SSRF with Filter Bypass via Open Redirection

## 🎯 Lab Overview

In this lab, I explored a Server-Side Request Forgery (SSRF) vulnerability within a product stock checker application. The objective was to manipulate the application into making internal requests to an admin interface, allowing us to delete a specific user.

## 🔍 Vulnerability Summary

Server-Side Request Forgery (SSRF) is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location. In this lab, the application fetches data from an internal system based on a user-supplied URL in the `stockApi` parameter. The developer has implemented a filter to restrict access to external resources. However, this filter can be bypassed using an open redirection vulnerability.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Visit a product and click "Check stock".
- **Tool**: Use Burp Suite to intercept the HTTP request.
- **Details**: The relevant request is a `POST` to `/product/stock` with parameters `productId` and `storeId`.

### 2. Identifying the Filter Restriction

- **Action**: Modify the `stockApi` parameter to:
    http://192.168.0.12:8080/admin
- **Observation**: The request is blocked, indicating that the application has implemented a filter to restrict access to external resources.

### 3. Discovering the Open Redirection

- **Action**: Click on the "next product" link and observe the behavior.
- **Observation**: The `path` parameter is placed into the `Location` header of a redirection response, resulting in an open redirection.

### 4. Deleting the User
- **Action**: Modify the `path` parameter to delete the target user:                
http%3A%2F%2Flocalhost%3a80%2523@stock.weliketoshop.net%3A8080%2Fadmin/delete?username=carlos
- same command but simpler for human understanding below :
http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
- **Observation**: The user `carlos` is deleted, solving the lab.

## 🧠 Key Takeaways

- **Understanding SSRF**: SSRF vulnerabilities occur when an application fetches data from a user-supplied URL. By manipulating this URL, attackers can direct the application to make requests to internal resources.
- **Bypassing Filters**: Filters that restrict access to external resources can often be bypassed using open redirection vulnerabilities.
- **Importance of Input Validation**: Proper validation and sanitization of user inputs are crucial to prevent SSRF vulnerabilities.

## ⚠️ Challenges Faced

- **Identifying the Vulnerable Parameter**: Determining which parameter was used to fetch data from the internal system required careful analysis of the application's behavior.
- **Bypassing Filters**: Accessing the admin interface was blocked by filters, necessitating the use of open redirection to bypass these controls.

## 💡 Tips for Others

- **Thorough Testing**: Test all user inputs, including hidden fields and HTTP headers, for potential SSRF vulnerabilities.
- **Use of Burp Suite**: Leverage Burp Suite's Proxy and Repeater tools to intercept, modify, and resend requests efficiently.
- **Learning Resources**: Refer to PortSwigger's [SSRF documentation](https://portswigger.net/web-security/ssrf) for in-depth understanding and examples.

## 📸 Screenshots

1. **Intercepted Request**  
 ![Intercepted Request](path/to/intercepted_request.png)

2. **Modified Request**  
 ![Modified Request](path/to/modified_request.png)

3. **Admin Interface**  
 ![Admin Interface](path/to/admin_interface.png)

4. **Response Output**  
 ![Response Output](path/to/response_output.png)
