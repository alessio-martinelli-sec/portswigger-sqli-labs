# SQL Injection – UNION (Column Count)

## 📌 Lab

PortSwigger: *SQL injection UNION attack, determining the number of columns returned by the query*

---

## 📌 Overview

This lab contains a SQL Injection vulnerability in the product category filter.

The goal is to determine the number of columns returned by the query using a UNION attack.

---

## 🔍 Injection Point

The injection point is the `category` parameter.

---

## 🧪 Testing

To confirm SQL Injection, I used a single quote:

```
'
```

This caused an error, indicating a possible injection point.

Then I tested boolean conditions:

```
' OR 1=1 --
' AND 1=2 --
```

* `' OR 1=1 --` → returns all products
* `' AND 1=2 --` → returns no products

This confirms a SQL Injection vulnerability.

---

## 🧪 Determining Column Count

To determine the number of columns, I used the `ORDER BY` clause:

```
' ORDER BY 1 --
' ORDER BY 2 --
' ORDER BY 3 --
' ORDER BY 4 --
```

* `ORDER BY 1` → valid
* `ORDER BY 2` → valid
* `ORDER BY 3` → valid
* `ORDER BY 4` → error

This indicates that the query returns **3 columns**.

---

## 💣 Exploit

To confirm the column count, I used a UNION SELECT with NULL values:

```
' UNION SELECT NULL, NULL, NULL --
```

This successfully returned a valid response, confirming that the query has 3 columns.

---

## 🧠 Explanation

The application likely executes a query like:

```sql
SELECT name, description, price FROM products WHERE category = 'Lifestyle';
```

Using `ORDER BY`, we determine how many columns exist.

Using `UNION SELECT`, we must match the exact number of columns in order to avoid errors.

The UNION operator requires the same number of columns in both queries, otherwise an error occurs.

`NULL` values are used because they are compatible with most data types.

---

## 🎯 Impact

* Ability to perform UNION-based SQL Injection
* Foundation for extracting data from other tables

---

## 🛠️ Fix

* Use prepared statements (parameterized queries)
* Avoid concatenating user input into SQL queries
* Validate user input

---

## 🧠 Notes

Determining the number of columns is the first step in UNION-based SQL Injection.
