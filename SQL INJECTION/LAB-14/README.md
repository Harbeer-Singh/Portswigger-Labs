# 🧪 PortSwigger Lab: Blind SQL Injection with Time Delays

## 🎯 Lab Overview

This lab demonstrates a blind SQL injection vulnerability in a web application that uses a tracking cookie (`TrackingId`) for analytics purposes. The application performs a SQL query using the value of this cookie, but the results are not returned to the user. Instead, the application executes the query synchronously, allowing attackers to infer information based on the time delay in the application's response.

---

## 🔍 Key Concepts and Learnings

### ✅ Understanding Blind SQL Injection with Time Delays

- **Blind SQL Injection**: Occurs when an application is vulnerable to SQL injection, but it does not display the results of the query or any error messages.

- **Time-Based Blind SQL Injection**: A type of blind SQL injection where attackers induce a time delay in the application's response to infer information about the database.

### ✅ Exploiting Time Delays

- **Inducing Delays**: By injecting SQL code that causes the database to pause execution (e.g., using `pg_sleep(10)`), attackers can make the application delay its response.

- **Inferring Information**: The presence or absence of the delay indicates whether a certain condition is true or false, allowing attackers to infer information about the database structure or content.

### ✅ Practical Application

- **Testing Conditions**: By systematically testing different conditions (e.g., `AND 1=1`, `AND 1=2`), attackers can determine which conditions are true based on the application's response time.

- **Extracting Data**: Once the presence of a condition is confirmed, attackers can modify the injected SQL to extract specific data from the database.

---

## 🛡️ Mitigation Strategies

To prevent blind SQL injection vulnerabilities, developers should:

- **Use Prepared Statements**: Ensure that SQL queries are parameterized, separating data from code.

- **Implement Input Validation**: Validate and sanitize all user inputs to prevent malicious data from being processed.

- **Employ Web Application Firewalls (WAFs)**: Use WAFs to detect and block SQL injection attempts.

- **Conduct Regular Security Audits**: Regularly test and review applications for vulnerabilities.

---

## 🧠 Conclusion

This lab illustrates how blind SQL injection vulnerabilities can be exploited using time delays to infer information about a database. Understanding and mitigating such vulnerabilities are crucial for maintaining the security of web applications.

---

## 📸 Screenshots

1. **Proof Of Completion**  
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-14/image/1.png)

