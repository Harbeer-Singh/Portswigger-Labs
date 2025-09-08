# 🧪 PortSwigger Lab: Blind SQL Injection — With Conditional errors

## 🎯 Lab Overview

This lab focuses on exploiting a blind SQL injection vulnerability in a web application where the response does not directly reveal query results.  
The application relies on a tracking cookie (`TrackingId`) whose value is used in a SQL query.  
The outcome of the query is only observable via subtle differences in the application's response, such as the presence or absence of a "Welcome back" message.

The goal of this lab is to extract sensitive information, specifically the administrator's password, character by character using boolean-based blind SQL injection techniques.

---

## 🔍 Key Concepts and Learnings

### ✅ Understanding Blind SQL Injection

- Blind SQL injection occurs when the application does not return error messages or query results, forcing attackers to infer information based on indirect indicators.
- This technique requires patience and precision, as attackers must carefully construct queries that test one piece of data at a time.

### ✅ Boolean-Based Data Extraction

- Attackers use boolean conditions to test hypotheses about the data.
- For example, by checking whether a certain character exists at a specific position, attackers can gradually uncover the full value of sensitive information.
- The application’s response reveals whether the condition was true or false.

### ✅ Extracting Data Without Direct Feedback

- Even without direct output, attackers can systematically extract data by observing behavioral changes such as response content, error messages, or timing differences.
- This method highlights how subtle vulnerabilities can be exploited to retrieve confidential data.

### ✅ Importance of Patience and Automation

- Manually extracting data character by character can be tedious and time-consuming.
- Automation tools and scripts can greatly accelerate the process by iterating through character sets and positions efficiently.

---

## 🛡️ Mitigation Strategies

To defend against blind SQL injection attacks, developers should:

- **Use Parameterized Queries**  
  Ensure that inputs are treated as data rather than executable code.

- **Implement Input Validation and Escaping**  
  Validate inputs rigorously and escape special characters to prevent injection.

- **Employ Web Application Firewalls (WAFs)**  
  Use WAFs to detect and block suspicious payloads that attempt to exploit injection vulnerabilities.

- **Monitor Application Responses**  
  Keep track of abnormal application behavior and investigate potential vulnerabilities proactively.

---

## 🧠 Conclusion

This lab illustrates how attackers can extract highly sensitive information from a web application without direct access to query results.  
By leveraging blind SQL injection techniques and analyzing subtle behavioral cues, attackers can uncover confidential data one character at a time.  
It underscores the critical importance of secure coding practices, thorough validation, and defensive mechanisms to protect applications from such threats.

---

## 📸 Screenshots

1. **Proof OF Completion**  
   ![Initial Response](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-12/image/1.png)


