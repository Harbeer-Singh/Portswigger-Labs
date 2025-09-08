# 🧪 PortSwigger Lab: Basic SSRF against the Local Server

## 🎯 Lab Overview

In this lab, I explored a Server-Side Request Forgery (SSRF) vulnerability within a product stock checker application. The objective was to manipulate the application into making an internal request to the admin interface, allowing us to delete a specific user.

## 🔍 Vulnerability Summary

Server-Side Request Forgery (SSRF) is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location. In this lab, the application fetches data from an internal system based on a user-supplied URL in the `stockApi` parameter. By manipulating this parameter, we can direct the application to access internal resources, such as the admin interface.

## 🛠️ Exploitation Steps

### 1. Accessing the Admin Interface

- **Action**: Attempt to directly access the admin page by navigating to `/admin`.
- **Observation**: The request is blocked, indicating that direct access to the admin interface is restricted.

### 2. Intercepting the Stock Check Request

- **Action**: Visit a product and click "Check stock".
- **Tool**: Use Burp Suite to intercept the HTTP request.
- **Details**: The relevant request is a `POST` to `/product/stock` with parameters `productId` and `storeId`.

### 3. Redirecting the Request to the Admin Interface

- **Action**: Modify the `stockApi` parameter in the intercepted request to:
- **Explanation**: This directs the application to make a request to the internal admin interface.

### 4. Identifying the User Deletion URL

- **Action**: Submit the modified request and observe the response.
- **Details**: The response contains the HTML of the admin interface, revealing the URL to delete a user:

### 5. Executing the SSRF Attack

- **Action**: Modify the `stockApi` parameter to:
- **Explanation**: This causes the application to make a request to the internal admin interface, deleting the user `carlos`.

## COMMAND
- http%3a//localhost/admin/delete?username=carlos

## 🧠 Key Takeaways

- **Understanding SSRF**: SSRF vulnerabilities occur when an application fetches data from a user-supplied URL. By manipulating this URL, attackers can direct the application to make requests to internal resources.
- **Accessing Internal Resources**: Even if direct access to internal resources is restricted, SSRF can be used to bypass these restrictions.
- **Importance of Input Validation**: Proper validation and sanitization of user inputs are crucial to prevent SSRF vulnerabilities.

## ⚠️ Challenges Faced

- **Identifying the Vulnerable Parameter**: Determining which parameter was used to fetch data from the internal system required careful analysis of the application's behavior.
- **Bypassing Access Controls**: Accessing the admin interface was blocked, necessitating the use of SSRF to bypass these controls.

## 💡 Tips for Others

- **Thorough Testing**: Test all user inputs, including hidden fields and HTTP headers, for potential SSRF vulnerabilities.
- **Use of Burp Suite**: Leverage Burp Suite's Proxy and Repeater tools to intercept, modify, and resend requests efficiently.
- **Learning Resources**: Refer to PortSwigger's [SSRF documentation](https://portswigger.net/web-security/ssrf) for in-depth understanding and examples.

## 📸 Proof Of Completion

1. ****  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SSRF/LAB-1/images/1.png)

