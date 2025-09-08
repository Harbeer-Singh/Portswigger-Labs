# 🧪 PortSwigger Lab: SQL Injection — UNION Attack — Finding a Column Containing Text

## 🎯 Lab Overview

In this lab, I explored an SQL injection vulnerability within a product category filter in a web application. The objective was to perform a UNION-based SQL injection attack to retrieve data from other tables by identifying a column that can accept and display string data.

## 🔍 Vulnerability Summary

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. In this lab, the application is vulnerable to SQL injection in the product category filter. By exploiting this vulnerability, I was able to execute arbitrary SQL queries, including those that retrieve data from other tables.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Use Burp Suite to intercept and modify the request that sets the product category filter.
- **Details**: The vulnerable parameter is the `category` parameter in the `GET` request.

### 2. Determining the Number of Columns

- **Action**: Submit the following payload to determine the number of columns in the original query:
'+UNION+SELECT+NULL,NULL,NULL--'
- **Observation**: The application returns an error indicating the number of columns in the original query. Adjust the number of `NULL` values until the error disappears, confirming the correct number of columns.

### 3. Identifying a Column with a Useful Data Type

- **Action**: Submit the following payload to identify a column that can hold string data:
'+UNION+SELECT+NULL,'Test',NULL--'
- **Observation**: The application returns the string `'Test'` in the response, indicating that the second column can hold string data.

### 4. Retrieving the Random Value

- **Action**: Submit the following payload to retrieve the random value provided by the lab:
'UNION+SELECT+null,'ugdsOH',null--

pgsql
Copy code
- **Observation**: The application returns the string `'ugdsOH'` in the response, indicating that the second column can hold string data.

## 🧠 Key Takeaways

- **Understanding SQL Injection**: SQL injection vulnerabilities occur when an application includes unfiltered user input in an SQL query. This allows attackers to manipulate the query and execute arbitrary SQL commands.
- **Using UNION SELECT for Data Retrieval**: The `UNION SELECT` statement allows an attacker to combine the results of the original query with the results of a malicious query, enabling data retrieval from other parts of the database.
- **Identifying Columns with String Data Type**: To perform a successful UNION-based SQL injection attack, it is essential to identify a column that can accept and display string data. This can be achieved by injecting a string value and observing the application's response.

## ⚠️ Challenges Faced

- **Determining the Number of Columns**: Identifying the correct number of columns in the original query required trial and error with different numbers of `NULL` values.
- **Bypassing Filters**: The application may have implemented input filters to prevent SQL injection. Crafting payloads that bypass these filters required careful analysis and testing.

## 💡 Tips for Others

- **Use Burp Suite's Repeater Tool**: The Repeater tool in Burp Suite allows you to modify and resend HTTP requests, making it easier to test different payloads and observe the application's responses.
- **Consult the SQL Injection Cheat Sheet**: PortSwigger's [SQL Injection Cheat Sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) provides a comprehensive list of SQL injection techniques and payloads.
- **Practice in a Safe Environment**: Always practice SQL injection techniques in a controlled, legal environment, such as the PortSwigger Web Security Academy labs.

## 📸 Screenshots

1. **Request**  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-8/images/1.png)

2. **Complete**  
 ![Number of Columns](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-8/images/1.png)

