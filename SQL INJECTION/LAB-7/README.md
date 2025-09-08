# 🧪 PortSwigger Lab: SQL Injection — UNION Attack — Determining the Number of Columns

## 🎯 Lab Overview

In this lab, I explored an SQL injection vulnerability within a product category filter in a web application. The objective was to determine the number of columns returned by the original query using a UNION-based attack. This step is crucial for constructing a successful UNION-based SQL injection attack to retrieve data from other tables.

## 🔍 Vulnerability Summary

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. In this lab, the application is vulnerable to SQL injection in the product category filter. By exploiting this vulnerability, I was able to execute arbitrary SQL queries, including those that determine the number of columns returned by the original query.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Use Burp Suite to intercept and modify the request that sets the product category filter.
- **Details**: The vulnerable parameter is the `category` parameter in the `GET` request.

### 2. Injecting a UNION SELECT Payload

- **Action**: Modify the `category` parameter to inject a UNION SELECT payload with a single `NULL` value:
'+UNION+SELECT+NULL--'
- **Observation**: The application returns an error indicating that the number of columns in the original query does not match the number of columns in the injected query.

### 3. Payload
You can check the number of commands with the command 
`ORDER BY 1`
we find that we have 3 columns 
`'+UNION+SELECT+null,null,null--`


## 🧠 Key Takeaways

- **Understanding SQL Injection**: SQL injection vulnerabilities occur when an application includes unfiltered user input in an SQL query. This allows attackers to manipulate the query and execute arbitrary SQL commands.
- **Using UNION SELECT for Data Retrieval**: The `UNION SELECT` statement allows an attacker to combine the results of the original query with the results of a malicious query, enabling data retrieval from other parts of the database.
- **Determining the Number of Columns**: To perform a successful UNION-based SQL injection attack, it is essential to determine the number of columns returned by the original query. This can be achieved by incrementally adding `NULL` values to the injected query until the error disappears and the response includes the `NULL` values.

## ⚠️ Challenges Faced

- **Determining the Number of Columns**: Identifying the correct number of columns in the original query required trial and error with different numbers of `NULL` values.

## 💡 Tips for Others

- **Use Burp Suite's Repeater Tool**: The Repeater tool in Burp Suite allows you to modify and resend HTTP requests, making it easier to test different payloads and observe the application's responses.
- **Consult the SQL Injection Cheat Sheet**: PortSwigger's [SQL Injection Cheat Sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) provides a comprehensive list of SQL injection techniques and payloads.
- **Practice in a Safe Environment**: Always practice SQL injection techniques in a controlled, legal environment, such as the PortSwigger Web Security Academy labs.

## 📸 Screenshots

1. **Request**  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-7/images/1.png)

3. **Completed**  
 ![Column Mismatch](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-7/images/2.png)
