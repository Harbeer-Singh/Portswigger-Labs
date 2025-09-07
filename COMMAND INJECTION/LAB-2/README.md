
# 🧪 PortSwigger Lab: Blind OS Command Injection with Time Delays

## 🎯 Lab Overview

In this lab, I explored a blind OS command injection vulnerability within a feedback submission form. Unlike standard OS command injection, the application does not return the output of the executed command. Instead, it exhibits a time delay when a command is executed, allowing us to infer successful exploitation based on the response time.

## 🔍 Vulnerability Summary

Blind OS command injection occurs when an application executes user-supplied input as part of a system command, but does not return the command's output. Instead, the application's response time can be used to infer whether the command was executed. In this lab, the application executes a shell command containing the user-supplied details, and the output from the command is not returned in the response. To solve the lab, we exploit this vulnerability to cause a 10-second delay.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Open the lab and navigate to the feedback submission form.
- **Tool**: Use Burp Suite to intercept the HTTP request when submitting feedback.
- **Details**: The relevant request is a `POST` to `/feedback/submit` with parameters `csrf`, `name`, `email`, `subject`, and `message`.

### 2. Identifying the Vulnerable Parameter

- **Action**: Analyze the intercepted request to identify which parameter is being passed to a shell command.
- **Observation**: The `email` parameter appears to be a suitable candidate for injection.

### 3. Sending the Malicious Request

- **Action**: Send the modified request using Burp Suite's Repeater tool.

### 4. Observing the Response

- **Action**: Observe the server's response time.
- **Expected Outcome**: The response should take approximately 10 seconds to return, indicating that the injected command was executed.

## 🧠 Key Takeaways

- **Blind OS Command Injection**: This technique allows attackers to infer the execution of system commands based on the application's response time, even when the command's output is not returned.
- **Input Validation**: Proper validation and sanitization of user inputs are crucial to prevent command injection vulnerabilities.
- **Security Testing**: Regularly test applications for blind OS command injection vulnerabilities using tools like Burp Suite.

## ⚠️ Challenges Faced

- **Identifying Injection Points**: Determining which parameters were vulnerable to command injection required careful analysis of the application's behavior.
- **Bypassing Input Filters**: Some input filters attempted to block certain characters, necessitating the use of alternative encoding or command syntax.

## 💡 Tips for Others

- **Thorough Testing**: Test all user inputs, including hidden fields and HTTP headers, for potential injection points.
- **Use of Burp Suite**: Leverage Burp Suite's Proxy and Repeater tools to intercept, modify, and resend requests efficiently.
- **Learning Resources**: Refer to PortSwigger's [OS Command Injection documentation](https://portswigger.net/web-security/os-command-injection) for in-depth understanding and examples.

### 3. Crafting the Payload
**%26ping+-c+10+127.0.0.1%26**

## 📸 Screenshots

1. **Intercepted Request**: ![Intercepted Request](path/to/intercepted_request.png)
2. **Modified Request**: ![Modified Request](path/to/modified_request.png)
3. **Response Output**: ![Response Output](path/to/response_output.png)

