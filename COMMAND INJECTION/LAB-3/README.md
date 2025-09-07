 # 🧪 PortSwigger Lab: Blind OS Command Injection with Output Redirection

## 🎯 Lab Overview

In this lab, I explored a blind OS command injection vulnerability within a feedback submission form. Unlike standard OS command injection, the application does not return the output of the executed command. Instead, it allows for output redirection to a file, enabling us to retrieve the command's output by accessing the file later.

## 🔍 Vulnerability Summary

Blind OS command injection occurs when an application executes user-supplied input as part of a system command, but does not return the command's output. Instead, the application's response time can be used to infer whether the command was executed. In this lab, the application executes a shell command containing the user-supplied details, and the output from the command is not returned in the response. However, we can use output redirection to capture the output from the command.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Open the lab and navigate to the feedback submission form.
- **Tool**: Use Burp Suite to intercept the HTTP request when submitting feedback.
- **Details**: The relevant request is a `POST` to `/feedback/submit` with parameters `csrf`, `name`, `email`, `subject`, and `message`.

### 2. Identifying the Vulnerable Parameter

- **Action**: Analyze the intercepted request to identify which parameter is being passed to a shell command.
- **Observation**: The `email` parameter appears to be a suitable candidate for injection.

### 3. Crafting the Payload

- **1**: ieCRwES3iFAZ365uCghorxU97S1Bo63i&name=abcd&email=whoami>/var/www/images/output.txt&subject=abcd&message=dcba
- **2**: GET /image?filename=whoami.txt


### 4. Sending the Malicious Request

- **Action**: Send the modified request using Burp Suite's Repeater tool.

### 5. Retrieving the Output

- **Action**: Access the file containing the command's output.
- **Details**: The application serves images from the `/var/www/images/` directory. By navigating to the following URL, we can view the contents of the `output.txt` file:
- Replace `<lab-url>` with the actual URL of the lab.

## 🧠 Key Takeaways

- **Blind OS Command Injection**: This technique allows attackers to infer the execution of system commands based on the application's response time, even when the command's output is not returned.
- **Output Redirection**: By redirecting the output of a command to a file, we can retrieve the command's output later, facilitating the exploitation of blind OS command injection vulnerabilities.
- **Input Validation**: Proper validation and sanitization of user inputs are crucial to prevent command injection vulnerabilities.
- **Security Testing**: Regularly test applications for blind OS command injection vulnerabilities using tools like Burp Suite.

## ⚠️ Challenges Faced

- **Identifying Injection Points**: Determining which parameters were vulnerable to command injection required careful analysis of the application's behavior.
- **Bypassing Input Filters**: Some input filters attempted to block certain characters, necessitating the use of alternative encoding or command syntax.

## 💡 Tips for Others

- **Thorough Testing**: Test all user inputs, including hidden fields and HTTP headers, for potential injection points.
- **Use of Burp Suite**: Leverage Burp Suite's Proxy and Repeater tools to intercept, modify, and resend requests efficiently.
- **Learning Resources**: Refer to PortSwigger's [OS Command Injection documentation](https://portswigger.net/web-security/os-command-injection) for in-depth understanding and examples.

## 📸 Screenshots

1. **Intercepted Request**  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/COMMAND%20INJECTION/LAB-3/images/1.png)

2. **Modified Request**  
 ![Modified Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/COMMAND%20INJECTION/LAB-3/images/2.png)

3. **Output**  
 ![Response Output](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/COMMAND%20INJECTION/LAB-3/images/3.png)
