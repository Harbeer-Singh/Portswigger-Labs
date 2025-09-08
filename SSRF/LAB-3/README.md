http%3a//127.1/%2561dmin // double url encoded a = %2561
http%3a//127.1/%2561dmin/delete?username=carlos
# 🧪 PortSwigger Lab: SSRF with Blacklist-Based Input Filter

## 🎯 Lab Overview

In this lab, I explored a Server-Side Request Forgery (SSRF) vulnerability within a product stock checker application. The objective was to manipulate the application into making internal requests to an admin interface, allowing us to delete a specific user.

## 🔍 Vulnerability Summary

Server-Side Request Forgery (SSRF) is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location. In this lab, the application fetches data from an internal system based on a user-supplied URL in the `stockApi` parameter. The developer has implemented a blacklist-based input filter to prevent SSRF attacks by blocking certain keywords like `localhost` and `127.0.0.1`. However, these filters can often be bypassed using various techniques.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Visit a product and click "Check stock".
- **Tool**: Use Burp Suite to intercept the HTTP request.
- **Details**: The relevant request is a `POST` to `/product/stock` with parameters `productId` and `storeId`.

### 2. Identifying the Blacklist Filter

- **Action**: Modify the `stockApi` parameter to:
    http://127.0.0.1/
- **Observation**: The request is blocked, indicating that the application has implemented a blacklist filter to prevent access to certain internal resources.

### 3. Bypassing the Blacklist Filter

- **Action**: Modify the `stockApi` parameter to:
http://127.1/
- **Observation**: The request is successful, indicating that the filter can be bypassed by using alternative representations of the IP address.

### 4. Accessing the Admin Interface

- **Action**: Modify the `stockApi` parameter to:
    http://127.1/admin
- **Observation**: The request is blocked again, indicating that the application has implemented another filter to prevent access to the admin interface.

### 5. Bypassing the Admin Interface Filter

- **Action**: Modify the `stockApi` parameter to:
http%3a//127.1/%2561dmin // double url encoded a = %2561
- **Explanation**: The `%2561` is the double URL-encoded representation of the character `a`, which bypasses the filter.
- **Observation**: The request is successful, granting access to the admin interface.

### 6. Deleting the User

- **Action**: Navigate to the delete user page by modifying the `stockApi` parameter to:
    http%3a//127.1/%2561dmin/delete?username=carlos
- **Observation**: The user `carlos` is deleted, solving the lab.

## 🧠 Key Takeaways

- **Understanding SSRF**: SSRF vulnerabilities occur when an application fetches data from a user-supplied URL. By manipulating this URL, attackers can direct the application to make requests to internal resources.
- **Weak Anti-SSRF Defenses**: Blacklist-based input filters can often be bypassed using various techniques, such as using alternative representations of IP addresses or double URL encoding.
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
 ![Intercepted Request](path/to/intercepted_request.png)

4. **Completed**  
 ![Response Output](path/to/response_output.png)
