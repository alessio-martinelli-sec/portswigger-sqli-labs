# SQL Injection – WHERE Clause (Hidden Data)

## 📌 Lab

PortSwigger: *SQL injection vulnerability in WHERE clause allowing retrieval of hidden data*

---

## 📌 Overview

This lab is vulnerable to SQL Injection in the product category filter.

The goal is to retrieve hidden (unreleased) products.

---

## 🔍 Injection Point

The injection point is the `category` parameter (e.g. `category=Lifestyle`).

---

## 🧪 Testing

To test for SQL Injection, I used a single quote:

```
'
```

This caused a SQL error, indicating a possible injection point.

Then I tested boolean conditions:

```
' OR 1=1 --
' AND 1=2 --
```

* `' OR 1=1 --` → returns all products (including hidden ones)
* `' AND 1=2 --` → returns no products

This confirms a boolean-based SQL Injection vulnerability.

---

## 💣 Exploit

```
' OR 1=1 --
```

---

## 🧠 Explanation

The application likely executes a query like:

```sql
SELECT * FROM products WHERE category = 'Lifestyle' AND released = 1;
```

After injection:

```sql
SELECT * FROM products WHERE category = '' OR 1=1 -- ' AND released = 1;
```

The `--` sequence comments out the rest of the query, removing the `released = 1` condition.

The condition `OR 1=1` is always true, so the query returns all rows.

This bypasses the application logic and exposes hidden products.

---

## 🎯 Impact

* Bypass of application logic
* Access to hidden or unreleased data

---

## 🛠️ Fix

* Use prepared statements (parameterized queries)
* Avoid concatenating user input into SQL queries
* Validate and sanitize user input

---

## 🧠 Notes

This is a classic example of boolean-based SQL Injection.
