# 🧪 PortSwigger Lab: Blind SQL Injection with Conditional Responses

## 🎯 Lab Overview

This lab presents a blind SQL injection vulnerability within a web application that utilizes a tracking cookie (`TrackingId`) for analytics purposes.  
The application performs a SQL query using the value of this cookie, but the results are not directly returned to the user.  
Instead, the application conditionally displays a "Welcome back" message if the query returns any rows, providing a subtle indicator of the query's outcome.

---

## 🔍 Key Concepts and Learnings

### ✅ Understanding Blind SQL Injection

- Blind SQL injection occurs when an application is vulnerable to SQL injection, but it does not display the results of the query or any error messages.  
- Instead, the attacker must infer information based on the application's behavior, such as changes in the response content or structure.

### ✅ Leveraging Conditional Responses

- In this lab, the application's response varies based on the result of the SQL query.  
- By crafting specific conditions within the injected SQL, an attacker can determine whether the condition is true or false by observing the presence or absence of the "Welcome back" message.

### ✅ Inferring Information Through Boolean Conditions

- By systematically testing different boolean conditions, an attacker can infer the existence of certain database elements, such as tables or specific rows, based on the application's response.

### ✅ Extracting Data Without Direct Output

- Even without direct access to the query results, attackers can extract information by constructing conditions that reveal the presence of specific data.  
- For instance, checking if a certain table or user exists can be achieved by observing the application's response to specific boolean conditions.

---

## 🛡️ Mitigation Strategies

To prevent blind SQL injection vulnerabilities, developers should:

- **Implement Prepared Statements**  
  Use parameterized queries to ensure that user inputs are treated as data, not executable code.

- **Employ Stored Procedures**  
  Encapsulate SQL logic within stored procedures to reduce direct manipulation of SQL queries.

- **Use ORM Frameworks**  
  Leverage Object-Relational Mapping (ORM) frameworks that abstract SQL queries and reduce direct interaction with the database.

- **Conduct Regular Security Audits**  
  Perform regular security assessments and code reviews to identify and rectify potential vulnerabilities.

---

## 🧠 Conclusion

This lab underscores the importance of understanding and mitigating blind SQL injection vulnerabilities.  
By recognizing how attackers can infer information through conditional responses, developers can better safeguard their applications against such attacks.  
Implementing robust input validation and employing secure coding practices are essential steps in protecting against SQL injection threats.

---

## 📸 Screenshots

1. **Proof OF Completion**  
   ![Initial Application Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-11/images/1.png)
