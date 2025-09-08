# 🧪 PortSwigger Lab: SQL Injection with Filter Bypass via XML Encoding

## 🎯 Lab Overview

This lab explores an SQL injection vulnerability that is protected by basic input filters, making it more difficult to exploit. The application uses a vulnerable parameter that filters out certain keywords and characters commonly associated with SQL injection. However, by encoding input in XML format, attackers can bypass these filters and inject malicious SQL statements to retrieve sensitive data.

The goal of this lab is to exploit the filter by crafting a payload that bypasses the restrictions, allowing the attacker to manipulate the database query and extract information such as the administrator's password.

---

## 🔍 Key Concepts and Learnings

### ✅ Understanding Filter Bypass Techniques

- **Input Filters**:  
  Applications often implement input filters to block suspicious characters or patterns. However, these filters can be bypassed by using alternate encoding schemes or representations.

- **XML Encoding**:  
  XML encoding replaces certain characters with their entity equivalents (e.g., `<` becomes `&lt;`), allowing attackers to evade filters that look for specific characters in their raw form.

### ✅ Exploiting SQL Injection with Encoding

- By encoding payloads using XML entities, attackers can submit inputs that appear safe to the filter but are interpreted by the server as malicious SQL.

- This technique demonstrates how encoding schemes can be abused to circumvent poorly implemented security controls.

### ✅ Extracting Data After Bypass

- Once the payload successfully bypasses the filter, it can be used to manipulate the database query and extract sensitive information such as usernames and passwords.

- Understanding the underlying structure of the SQL query helps craft payloads that target specific data fields.

---

## 📦 Payload Used

**Injected Payload:**  1 UNION SELECT password from users where username ='administrator'--

 ---
 
 ## 🛡️ Mitigation Strategies

To prevent filter bypass via encoding techniques, developers should:

**Implement Proper Input Validation**
Avoid relying solely on keyword blocking or pattern matching. Instead, use comprehensive input validation and sanitation.

**Use Prepared Statements**
Parameterized queries ensure that user input is treated as data rather than executable SQL.

**Avoid Security Through Obscurity**
Relying on input filters without understanding underlying vulnerabilities can give a false sense of security.

**Conduct Security Testing**
Regularly test applications against advanced attack techniques, including encoding-based bypass methods.

---

### 🧠 Conclusion

This lab highlights how attackers can bypass simple input filters using encoding techniques such as **XML entities**. It reinforces the importance of using secure coding practices like prepared statements and proper validation, rather than relying on superficial protections. Understanding how attackers can manipulate inputs at different layers helps developers better secure their applications against sophisticated threats.

---

## 📸 Screenshots

1. **Proof OF Completion`**  
   ![Intercepted Request](https://github.com/Harbeer-Singh/Portswigger-Labs/blob/main/SQL%20INJECTION/LAB-19/image/1.png)



