✅ PortSwigger Lab: SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

🔗 Lab Link: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

⚙️ Difficulty: Easy
📂 Category: Web Security → SQL Injection
📅 Completion Date: (Add your completion date here)

📚 Description

This lab demonstrates an SQL Injection vulnerability in a web application’s category parameter. The objective is to retrieve hidden (unreleased) product data by injecting a crafted payload, bypassing application restrictions. It highlights the dangers of improper input sanitization and the potential for unauthorized data exposure in poorly secured applications.

⚡ Key Takeaways

What is SQL Injection?
A vulnerability where malicious SQL code is injected into input fields or URL parameters, allowing attackers to manipulate backend database queries.

Vulnerable Parameter:
The category parameter is used directly in the SQL query without proper sanitization or parameterization.

Payload Strategy:
Payload Example:

' OR '1'='1' --


This bypasses the restriction by making the WHERE clause always true.

Impact:
All product data, including hidden or unreleased products, was exposed due to the injection.

🧱 Commands
GET /filter?category=' OR 1=1-- HTTP/2


(Add your additional commands here as necessary)

📸 Screenshots


(Insert your first screenshot here)


(Insert your second screenshot here)


(Insert your third screenshot showing the hidden data)

📝 What I Learned

The critical role of parameterized queries in preventing SQL Injection attacks.

Why input sanitization alone is insufficient without proper query handling.

Practical experience using Burp Suite to intercept and manipulate HTTP requests for vulnerability assessment.

Real-world consequences of SQL Injection: unauthorized data access and potential system compromise.

🔐 Mitigation Techniques

Implement prepared statements with parameterized queries in backend applications.

Apply strong input validation and sanitization at every point of user input.

Deploy a Web Application Firewall (WAF) to detect and block common SQL Injection attack patterns.

👤 Author

Harbeer-Singh
