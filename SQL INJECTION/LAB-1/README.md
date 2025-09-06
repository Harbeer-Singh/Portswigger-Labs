# ✅ PortSwigger Lab: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

**🔗 Lab Link:**  
[https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)

**⚙️ Difficulty:** Easy  
**📂 Category:** Web Security → SQL Injection  
---

## 📚 Description

This lab demonstrates an **SQL Injection vulnerability** in a web application’s category parameter.  
The goal was to retrieve hidden (unreleased) product data by injecting a crafted payload that bypassed application restrictions.  
It highlights the dangers of improper input sanitization and shows how this can lead to unauthorized data exposure.

---

## ⚡ Key Takeaways

- **What is SQL Injection?**  
  SQL Injection is a critical vulnerability where malicious SQL code is injected into input fields or URL parameters, allowing attackers to manipulate database queries.

- **Vulnerable Parameter:**  
  The `category` parameter was directly used in the SQL query without sanitization.

---

## ⚡ Explanation of Payload Behavior and Impact

This bypasses the condition by making the WHERE clause always true.

**Impact:**  
The injection allowed retrieval of all products, including hidden/unreleased items.

---

## 🧱 Commands

*GET /filter?category=' OR 1=1-- HTTP/2*

---

## 📸 Screenshots

*![Screenshot 1 Alt Text](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-1/images/1.png)*

*![Screenshot 1 Alt Text](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-1/images/2.png)*


---

## 📝 What I Learned

- The importance of **parameterized queries** to prevent injection attacks.  
- Why **input sanitization alone isn’t sufficient** without proper query handling.  
- Practical usage of tools like **Burp Suite** to intercept and manipulate HTTP requests.  
- Real-world impact of SQL Injection: **data leakage and unauthorized access**.

---

## 🔐 Mitigation Techniques

- Use **prepared statements with parameterized queries** in backend applications.  
- Apply **strong input validation and sanitization** for every user input.  
- Deploy a **Web Application Firewall (WAF)** to block common injection patterns.

---

## 👤 Author

**Harbeer-Singh**
