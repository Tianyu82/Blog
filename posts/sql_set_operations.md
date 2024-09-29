---
title: '[SQL] Use UNION, INTERSECT, and EXCEPT in SQL'
date: '2024-09-25'
---

In this post, we'll explore how to use the `UNION`, `INTERSECT`, and `EXCEPT` operators in SQL. We'll work with two relations: `Own` and `Account`, and look at some specific queries involving accounts in different branches.

### The Schema

- **Own** relation:
  - `Customer_ID (PK)`
  - `Account_ID`
  
- **Account** relation:
  - `Account_ID (PK)`
  - `Branch_Name`
  - `Account_Balance`

---

## Question 1: Find the `Customer_ID`s of customers who have accounts in either the Robson or the Lonsdale branches (or both)

To find the `Customer_ID`s of customers who have accounts in **either** the Robson branch **or** the Lonsdale branch (or both), we use a simple `INNER JOIN` combined with a `WHERE` clause that filters for both branches.

```sql
SELECT O.Customer_ID
FROM Own O 
INNER JOIN Account A ON O.Account_ID = A.Account_ID
WHERE A.Branch_Name = 'Robson' OR A.Branch_Name = 'Lonsdale';
```

### Explanation:
- The `OR` operator ensures that we select `Customer_ID`s for customers who have accounts in either branch.

---

## Question 2: Find the `Customer_ID`s of customers who have accounts in **both** the Robson and the Lonsdale branches

Using `AND` in the `WHERE` clause will not generate the correct result for this query because no single account can exist in two different branches at the same time. Instead, we need to use the `INTERSECT` operator to find customers who appear in **both** results—one for the Robson branch and one for the Lonsdale branch.

```sql
SELECT O.Customer_ID
FROM Own O 
INNER JOIN Account A ON O.Account_ID = A.Account_ID
WHERE A.Branch_Name = 'Robson'
INTERSECT
SELECT O.Customer_ID
FROM Own O 
INNER JOIN Account A ON O.Account_ID = A.Account_ID
WHERE A.Branch_Name = 'Lonsdale';
```

### Explanation:
- The `INTERSECT` operator retrieves only the `Customer_ID`s that appear in both queries, meaning they have accounts in both the Robson and Lonsdale branches.

---

## Question 3: Find the `Customer_ID`s of customers who have accounts in the Robson branch but **not** in the Lonsdale branch

Here, we use the `EXCEPT` operator to find customers who have accounts in the Robson branch but **do not** have accounts in the Lonsdale branch.

```sql
SELECT O.Customer_ID
FROM Own O 
INNER JOIN Account A ON O.Account_ID = A.Account_ID
WHERE A.Branch_Name = 'Robson'
EXCEPT
SELECT O.Customer_ID
FROM Own O 
INNER JOIN Account A ON O.Account_ID = A.Account_ID
WHERE A.Branch_Name = 'Lonsdale';
```

### Explanation:
- `EXCEPT` subtracts the customers who have accounts in the Lonsdale branch from the list of customers who have accounts in the Robson branch.

---

## Challenge: Rewrite Question 3 using `EXISTS`

The `EXCEPT` query can also be written using the `EXISTS` clause. In this version, we check that a customer exists in the Robson branch, but does **not exist** in the Lonsdale branch for that same `Customer_ID`.

```sql
SELECT O.Customer_ID
FROM Own O 
INNER JOIN Account A ON O.Account_ID = A.Account_ID
WHERE A.Branch_Name = 'Robson'
AND NOT EXISTS (
    SELECT 1
    FROM Own O2 
    INNER JOIN Account A2 ON O2.Account_ID = A2.Account_ID
    WHERE A2.Branch_Name = 'Lonsdale' 
    AND O.Customer_ID = O2.Customer_ID
);
```

### Explanation:
- The subquery checks for the existence of accounts in the Lonsdale branch. The main query will only return `Customer_ID`s for which there are **no matching rows** in the subquery (i.e., those without Lonsdale accounts).

- Why is O.Customer_ID = O2.Customer_ID important?
This part of the query establishes the link between the outer query (Robson branch) and the subquery (Lonsdale branch). It ensures that for each customer found in Robson, we are specifically checking whether the same customer exists in the Lonsdale branch. Without this link, the NOT EXISTS check would compare all customers globally, leading to false exclusions or inclusions.

---
