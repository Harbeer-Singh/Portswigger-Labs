
# 🧪 PortSwigger Lab: SQL Injection — Querying the Database Version on MySQL and Microsoft

## 🎯 Lab Overview

In this lab, I explored an SQL injection vulnerability within a product category filter in a MySQL and Microsoft SQL Server-based web application. The objective was to manipulate the application into revealing the database version string, providing valuable information for further exploitation.

## 🔍 Vulnerability Summary

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. In this lab, the application is vulnerable to SQL injection in the product category filter. By exploiting this vulnerability, I was able to execute arbitrary SQL queries, including those that reveal the database version.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Use Burp Suite to intercept and modify the request that sets the product category filter.
- **Details**: The vulnerable parameter is the `category` parameter in the `GET` request.

### 2. Determining the Number of Columns

- **Action**: Submit the following payload to determine the number of columns in the original query:
' UNION SELECT NULL, NULL --
- **Observation**: The application returns an error indicating the number of columns in the original query. Adjust the number of `NULL` values until the error disappears, confirming the correct number of columns.

### 3. Identifying a Column with a Useful Data Type
- **Action**: Submit the following payload to identify a column that can hold string data:
' UNION SELECT 'abc', NULL --
- **Observation**: The application returns the string `'abc'` in the response, indicating that the first column can hold string data.

### 4. Retrieving the Database Version

- **Action**: Submit the following payload to retrieve the database version string:
'UNION+SELECT+%40%40version,null%23 
- **Observation**: The application returns the database version string in the response, revealing the MySQL or Microsoft SQL Server version.

## 🧠 Key Takeaways

- **Understanding SQL Injection**: SQL injection vulnerabilities occur when an application includes unfiltered user input in an SQL query. This allows attackers to manipulate the query and execute arbitrary SQL commands.
- **Database-Specific Syntax**: Different database management systems have unique syntax and functions. For example, `@@version` is used in MySQL and Microsoft SQL Server to retrieve the database version.
- **Using `UNION SELECT` for Data Retrieval**: The `UNION SELECT` statement allows an attacker to combine the results of the original query with the results of a malicious query, enabling data retrieval from other parts of the database.

## ⚠️ Challenges Faced

- **Determining the Number of Columns**: Identifying the correct number of columns in the original query required trial and error with different numbers of `NULL` values.
- **Bypassing Filters**: The application may have implemented input filters to prevent SQL injection. Crafting payloads that bypass these filters required careful analysis and testing.

## 💡 Tips for Others

- **Use Burp Suite's Repeater Tool**: The Repeater tool in Burp Suite allows you to modify and resend HTTP requests, making it easier to test different payloads and observe the application's responses.
- **Consult the SQL Injection Cheat Sheet**: PortSwigger's [SQL Injection Cheat Sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) provides a comprehensive list of SQL injection techniques and payloads.
- **Practice in a Safe Environment**: Always practice SQL injection techniques in a controlled, legal environment, such as the PortSwigger Web Security Academy labs.

## 📸 Screenshots

1. **Request**  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-4/images/1.png)

2. **Result**  
 ![Number of Columns](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-4/images/2.png)

