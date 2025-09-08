

# 🧪 PortSwigger Lab: Basic SSRF against another back-end system

## 🎯 Lab Overview

In this lab, I explored a Server-Side Request Forgery (SSRF) vulnerability within a product stock checker application. The objective was to manipulate the application into making internal requests to an admin interface, allowing us to delete a specific user.

## 🔍 Vulnerability Summary

Server-Side Request Forgery (SSRF) is a web security vulnerability that allows an attacker to cause the server-side application to make requests to an unintended location. In this lab, the application fetches data from an internal system based on a user-supplied URL in the `stockApi` parameter. By manipulating this parameter, we can direct the application to access internal resources, such as the admin interface.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Visit a product and click "Check stock".
- **Tool**: Use Burp Suite to intercept the HTTP request.
- **Command**:Burp Intruder - http%3a//192.168.0.X:8080/admin
- **Details**: The relevant request is a `POST` to `/product/stock` with parameters `productId` and `storeId`.

### 2. Modifying the `stockApi` Parameter

- **Action**: Change the `stockApi` parameter to:
    http://192.168.0.1:8080/admin
- **Explanation**: This directs the application to make a request to the internal admin interface.

### 3. Scanning for the Admin Interface

- **Action**: Send the modified request to Burp Intruder.
- **Details**: In Burp Intruder, highlight the final octet of the IP address (the number `1`) and click "Add §". In the Payloads side panel, change the payload type to "Numbers", and enter 1, 255, and 1 in the From, To, and Step boxes respectively. Click "Start attack".
- **Expected Outcome**: Burp Intruder will scan the internal `192.168.0.X` range for the admin interface on port `8080`.

### 4. Identifying the Admin Interface

- **Action**: Click on the Status column to sort it by status code ascending.
- **Expected Outcome**: You should see a single entry with a status of `200`, indicating the presence of the admin interface.

### 5. Deleting the User

- **Action**: Click on this request, send it to Burp Repeater, and change the path in the `stockApi` parameter to:
    http%3a//192.168.0.52:8080/admin/delete?username=carlos

- **Explanation**: This causes the application to make a request to the internal admin interface, deleting the user `carlos`.

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

## 📸 Screenshots

1. **Intercepted Request**  
 ![Intercepted Request](path/to/intercepted_request.png)

4. **Response Output**  
 ![Response Output](path/to/response_output.png)
