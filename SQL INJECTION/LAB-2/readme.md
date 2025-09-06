# ✅ PortSwigger Lab: SQL Injection Vulnerability Allowing Login Bypass

**🔗 Lab Link:**  
[https://portswigger.net/web-security/sql-injection/lab-login-bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → SQL Injection  
---

## 📚 Description

This lab demonstrates an **SQL Injection vulnerability** in a web application’s login functionality.  
The goal was to bypass authentication and gain access without valid credentials by injecting malicious SQL into the login fields.  
It highlights the risks associated with directly incorporating user input into SQL queries without proper sanitization or use of parameterized queries.

---

## ⚡ Key Takeaways

- **What is SQL Injection in Authentication?**  
  SQL Injection in login forms allows attackers to bypass authentication by manipulating the SQL query used to validate user credentials.

- **Vulnerable Parameters:**  
  Both `username` and `password` fields were vulnerable because they were directly inserted into the SQL query without sanitization.

---

## ⚡ Explanation of Payload Behavior and Impact

By injecting specially crafted input such as `' OR '1'='1` into the username or password fields, the authentication logic is altered so that the WHERE clause always evaluates to true.  
This causes the application to authenticate the attacker without verifying valid credentials.

**Impact:**  
The attacker is granted unauthorized access to the application as a valid user, potentially exposing sensitive functionalities and data.

---

## 🧱 Commands

username=administrator'OR+1=1

---

## 📸 Screenshots

*![Screenshot 1 Alt Text](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-2/images/1.png)*
*![Screenshot 1 Alt Text](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-2/images/2.png)*

---

## 📝 What I Learned

- How **SQL Injection can be used specifically to bypass authentication mechanisms**.  
- The dangers of directly embedding user inputs in SQL queries without validation or parameterization.  
- Practical usage of tools like **Burp Suite Intruder** to automate injection attempts and observe server responses.  
- The real-world impact of such vulnerabilities: **unauthorized access and privilege escalation risks**.

---

## 🔐 Mitigation Techniques

- Implement **prepared statements with parameterized queries** for all database interactions, including authentication checks.  
- Enforce **strict input validation rules** to reject unexpected or dangerous characters (e.g., quotes, comment symbols).  
- Limit error messages to avoid giving hints about the backend logic or database structure.  
- Deploy a **Web Application Firewall (WAF)** to detect and block common injection attempts.

---

## 👤 Author

**Harbeer-Singh**
