# ✅ PortSwigger Lab: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data  

**🔗 Lab Link:**  
[https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)  

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → SQL Injection  
**📅 Completion Date:** *(Add completion date here)*  

---

## 📚 Description  

This lab demonstrates an **SQL Injection vulnerability** in a web application’s category parameter. The objective is to retrieve hidden (unreleased) product data by injecting a crafted payload, bypassing application restrictions. It highlights the dangers of improper input sanitization and the potential for unauthorized data exposure in poorly secured applications.

---

## ⚡ Key Takeaways  

- **What is SQL Injection?**  
  A vulnerability where malicious SQL code is injected into input fields or URL parameters, allowing attackers to manipulate backend database queries.

- **Vulnerable Parameter:**  
  The `category` parameter is used directly in the SQL query without sanitization.

- **Payload Strategy:**  
  Example Payload:  
  ```sql
  ' OR '1'='1' --
