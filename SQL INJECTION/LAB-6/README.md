# 🧪 PortSwigger Lab: SQL Injection — Listing the Database Contents on Oracle

## 🎯 Lab Overview

In this lab, I explored an SQL injection vulnerability within a product category filter in an Oracle-based web application. The objective was to manipulate the application into revealing the database contents, including the usernames and passwords of all users.

## 🔍 Vulnerability Summary

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. In this lab, the application is vulnerable to SQL injection in the product category filter. By exploiting this vulnerability, I was able to execute arbitrary SQL queries, including those that list the database contents.

## 🛠️ Exploitation Steps

### 1. Intercepting the Request

- **Action**: Use Burp Suite to intercept and modify the request that sets the product category filter.
- **Details**: The vulnerable parameter is the `category` parameter in the `GET` request.

### 2. Determining the Number of Columns

- **Action**: Submit the following payload to determine the number of columns in the original query:
  `' UNION SELECT NULL, NULL --`
- **Observation**: The application returns an error indicating the number of columns in the original query. Adjust the number of `NULL` values until the error disappears, confirming the correct number of columns.

### 3. Identifying a Column with a Useful Data Type

- **Action**: Submit the following payload to identify a column that can hold string data:
  `'+UNION+SELECT+null,table_name+FROM+all_tables--`
- **Observation**: The application returns the string `'abc'` in the response, indicating that the first column can hold string data.

### 4. Listing the Tables in the Database

- **Action**: Submit the following payload to retrieve the list of tables in the database:
' UNION SELECT table_name, NULL FROM all_tables --

sql
Copy code
- **Observation**: The application returns the list of table names in the response.

### 5. Identifying the Table Containing User Credentials

- **Action**: Review the list of tables to identify the table that likely contains user credentials (e.g., `users`, `accounts`, etc.).

### 6. Listing the Columns in the User Table

- **Action**: Submit the following payload to retrieve the list of columns in the identified user table:
  `'+UNION+SELECT+null,column_name+FROM+all_tab_columns+where+table_name%3d'USERS_QMUHVR'--`
- **Observation**: The application returns the list of column names in the response.

### 7. Retrieving the Usernames and Passwords

- **Action**: Submit the following payload to retrieve the usernames and passwords from the user table:
  `'+UNION+SELECT+USERNAME_SPPHIP,PASSWORD_LPBCLO+FROM+USERS_QMUHVR--`
- **Observation**: The application returns the usernames and passwords in the response.

## 🧠 Key Takeaways

- **Understanding SQL Injection**: SQL injection vulnerabilities occur when an application includes unfiltered user input in an SQL query. This allows attackers to manipulate the query and execute arbitrary SQL commands.
- **Oracle-Specific Syntax**: In Oracle databases, every `SELECT` statement must specify a table to select `FROM`. The built-in `dual` table can be used for this purpose when performing `UNION SELECT` attacks.
- **Using `all_tables` and `all_tab_columns` Views**: The `all_tables` and `all_tab_columns` views in Oracle contain metadata about the database, including information about tables and columns, which can be useful for identifying and extracting data.

## ⚠️ Challenges Faced

- **Determining the Number of Columns**: Identifying the correct number of columns in the original query required trial and error with different numbers of `NULL` values.
- **Bypassing Filters**: The application may have implemented input filters to prevent SQL injection. Crafting payloads that bypass these filters required careful analysis and testing.

## 💡 Tips for Others

- **Use Burp Suite's Repeater Tool**: The Repeater tool in Burp Suite allows you to modify and resend HTTP requests, making it easier to test different payloads and observe the application's responses.
- **Consult the SQL Injection Cheat Sheet**: PortSwigger's [SQL Injection Cheat Sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) provides a comprehensive list of SQL injection techniques and payloads.
- **Practice in a Safe Environment**: Always practice SQL injection techniques in a controlled, legal environment, such as the PortSwigger Web Security Academy labs.

## 📸 Screenshots

1. **Payload**  
 ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-6/images/1.png)

 
 ![Number of Columns](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-6/images/2.png)


 ![Column Data Type](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-6/images/3.png)

4. **Completed**  
 ![List Tables](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-6/images/4.png)
