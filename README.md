# PortSwigger Web Security Labs

This repository contains my solutions and detailed write-ups for labs from the PortSwigger Web Security Academy.

## 📚 Labs
- Lab 4 – SQL Injection: Database version enumeration
- 
- # 🛡️ SQL Injection Lab – Database Type & Version Enumeration

## 📌 Overview

This repository contains a detailed write-up of a lab from the PortSwigger Web Security Academy focused on **SQL Injection (SQLi)**.

The lab demonstrates how an attacker can exploit a vulnerable parameter to:

* Identify the number of columns in a query
* Perform a `UNION SELECT` attack
* Extract the database type and version

---

## 🎯 Objective

The goal of this lab is to:

* Detect a SQL Injection vulnerability
* Determine the correct number of columns in the query
* Retrieve the database version information using a UNION-based attack

---

## 🌐 Vulnerable Endpoint

```http
GET /filter?category=Food+%26+Drink HTTP/1.1
```

The `category` parameter is vulnerable to SQL Injection.

---

## 🔍 Vulnerability Analysis

Initial testing with a single quote (`'`) caused a server error, indicating improper input handling and confirming a potential SQL Injection point.

### 🧪 Column Enumeration

To determine the number of columns returned by the query, a `UNION SELECT` payload was used:

```sql
' UNION SELECT NULL,NULL--
```

However, due to input filtering or syntax constraints, the comment sequence had to be URL-encoded:

```sql
NULL,NULL%23
```

This successfully executed, confirming that:

* The query returns **2 columns**

---

## ⚔️ Exploitation

After identifying the correct number of columns, the next step was to extract the database version.

### 🚀 Final Payload

```http
GET /filter?category=Food+%26+Drink' UNION SELECT @@version, NULL%23
```

---

## 🧠 Technical Breakdown

* `UNION SELECT`
  Combines the results of the original query with attacker-controlled output.

* `@@version`
  Returns the database version information.
  Works for both:

  * MySQL
  * Microsoft SQL Server

* `NULL`
  Used to match the number of columns required by the original query.

* `%23`
  URL-encoded form of `#`, used to comment out the rest of the SQL query.

---

## 📸 Proof of Concept

Screenshots of the exploitation process are available in the `screenshots/` directory, including:

* Intercepted request (Burp Suite)
* Payload injection
* Extracted database version in response

---

## 🛠️ Tools Used

* Burp Suite (Proxy & Repeater)
* Web Browser

---

## 📁 Project Structure

```
lab-4/
├── README.md
├── payloads.txt
├── exploit.py (optional)
└── screenshots/
```

---

## 🧠 Key Learnings

* Identifying SQL Injection via input behavior
* Determining column count using `UNION SELECT`
* Handling filtering with URL encoding (`%23`)
* Extracting sensitive database information
* Understanding cross-database payload compatibility

---

## 🛡️ Mitigation Strategies

To prevent SQL Injection vulnerabilities:

* Use **parameterized queries (prepared statements)**
* Avoid dynamic SQL query construction
* Implement strict **input validation and sanitization**
* Apply least privilege to database accounts
* Use Web Application Firewalls (WAF)

---

## ⚠️ Disclaimer

This repository is created for educational purposes only.
All labs and content belong to PortSwigger Web Security Academy.

---
## 👨‍💻 About Me
I am a cybersecurity enthusiast focusing on web application security and penetration testing.

This repository documents my hands-on practice on real-world vulnerabilities through PortSwigger labs.
## 🔗 References

* PortSwigger Web Security Academy
* OWASP Top 10 – Injection
