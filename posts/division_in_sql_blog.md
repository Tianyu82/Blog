---
title: '[SQL] Several ways to implement DIVISION in SQL'
date: '2024-09-29'
---

This division is not the arithmetic division operator (/). Don't get confused.

In SQL, the division operator is often needed in scenarios where you want to identify entities that meet all specified conditions across different datasets. Here are a few common examples:

- Identifying individuals who hold accounts at every bank within a specific city.
- Determining students who have enrolled in all required courses to qualify for graduation.

However, the `DIVISION` keyword is not natively supported in most SQL dialects, so we must implement it ourselves.

### Question: 
**How can we find the names and `customerID`s of customers who have an account in all branches?**

### Schema:
- **Customer**: `{customerID, firstName, lastName, birthDate, income}`
- **Account**: `{accNumber, type, balance, rate, branchName}`
- **Owns**: `{customerID, accNumber}`
- **Branch**: `{branchName, city, phone}`

### Analysis:
To solve this problem, we first identify all branches. Next, we determine which branches a particular customer has accounts with. If the two sets match, we return the customer.

#### (Set A)
```sql
SELECT B.branchName
FROM Branch B;
```

#### (Set B)
```sql
SELECT A.branchName
FROM Owns O 
INNER JOIN Account A ON A.accNumber = O.accNumber 
WHERE O.customerID = some_customerID; -- some_customerID serves as a placeholder correlating to the outer query
```

We can then use `EXCEPT` to verify if the two sets match. If they do, the `EXCEPT` statement will return null.

```sql
SELECT C.firstName, C.lastName, C.customerID
FROM Customer C
WHERE (
    NOT EXISTS (Set A EXCEPT Set B) 
);
```

### Final Version:
The final query implementation is as follows:

```sql
SELECT C.firstName, C.lastName, C.customerID
FROM Customer C
WHERE (
    NOT EXISTS (
        (SELECT B.branchName FROM Branch B)
        EXCEPT 
        (SELECT A.branchName
        FROM Owns O 
        INNER JOIN Account A ON A.accNumber = O.accNumber 
        WHERE O.customerID = C.customerID)
    ) 
);
```

In this query, we check for any branches that exist in Set A but not in Set B for each customer. If such branches do not exist, it indicates that the customer has accounts in all branches.
