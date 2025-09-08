# 🧪 PortSwigger Lab: Visible Error-Based SQL Injection

## 🎯 Lab Overview

This lab presents a SQL injection vulnerability within a web application that utilizes a tracking cookie (`TrackingId`) for analytics purposes. The application performs a SQL query using the value of this cookie, but the results of the query are not returned to the user. Instead, verbose error messages disclose detailed information about the SQL query, including the value of the `TrackingId` cookie, which can be exploited to infer sensitive data.

---

## 🔍 Key Concepts and Learnings

### ✅ Understanding Visible Error-Based SQL Injection

- **Visible error-based SQL injection** occurs when the application displays detailed database error messages that include information about the SQL query being executed.
- These verbose error messages can reveal the structure of the SQL query, the database type, and other sensitive information, which can be leveraged to craft further attacks.

### ✅ Exploiting Verbose Error Messages

- By manipulating the `TrackingId` cookie and observing the application's error messages, an attacker can infer the structure of the underlying SQL query.
- For example, appending a single quote (`'`) to the `TrackingId` value may cause a syntax error, and the resulting error message may disclose the full SQL query, including the value of the cookie.

### ✅ Extracting Sensitive Data

- Once the structure of the SQL query is understood, an attacker can modify the `TrackingId` value to include SQL payloads that extract sensitive information.
- For instance, by injecting a subquery that selects the password of the `administrator` user, the attacker can retrieve the password if the error message includes the result of the subquery.

---

## 🛡️ Mitigation Strategies

To prevent visible error-based SQL injection vulnerabilities, developers should:

- **Disable Detailed Error Messages**: Configure the application to display generic error messages to users and log detailed errors on the server side.
- **Implement Input Validation**: Validate and sanitize all user inputs to ensure they do not contain malicious SQL code.
- **Use Prepared Statements**: Employ parameterized queries or prepared statements to separate SQL code from data, preventing SQL injection attacks.
- **Conduct Regular Security Audits**: Regularly review and test the application's code and configurations to identify and mitigate potential vulnerabilities.

---

## 🧠 Conclusion

This lab demonstrates how verbose error messages can be exploited to perform visible error-based SQL injection attacks. By understanding the structure of SQL queries through error messages, attackers can craft payloads to extract sensitive information from the database. Implementing proper error handling, input validation, and secure coding practices are essential to protect applications from such vulnerabilities.

---

## 📸 Screenshots

1. **Initial Application Response**  
   ![Initial Application Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-13/image/1.png)
