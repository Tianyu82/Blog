---
title: '[SQL] Difference Between WHERE and HAVING in SQL'
date: '2024-09-12'
---

In SQL, both `WHERE` and `HAVING` clauses act as filters to remove unwanted records. However, they are used in different cases. This blog will explore the differences between these two clauses.

### Key Difference Between `WHERE` and `HAVING`

- **`WHERE` Clause**: Filters records at the row level (record, entry). It is applied **before** any grouping or aggregation occurs.  
- **`HAVING` Clause**: Filters records at the group level, **after** the `GROUP BY` clause has been executed and aggregate results have been computed.  

---

### Example: Using `WHERE` Clause

Given a table `sales` with the following columns: product_name, city, and sales_amount. For example, (laptop, Vancouver, 1400) represents a laptop was sold 1400 dollars in Vancouver.

If you want to calculate the total sales for each product in Vancouver, you can use the `WHERE` clause:

```sql
SELECT product_name, SUM(sales_amount) AS total_sales
FROM sales
WHERE city = 'Vancouver'
GROUP BY product_name;
```

- The `WHERE` clause filters records to include only sales from Vancouver.  
- The `GROUP BY` clause then groups these records by `product_name`, and `SUM(sales_amount)` calculates the total sales for each product.

---

### Example: Using `HAVING` Clause

Now, suppose you want to find all products in Vancouver with total sales below $3000 to optimize sales strategies. In this case, you use the `HAVING` clause:

```sql
SELECT product_name, SUM(sales_amount) AS total_sales
FROM sales
WHERE city = 'Vancouver'
GROUP BY product_name
HAVING SUM(sales_amount) < 3000;
```

- The `WHERE` clause first filters records to include only sales from Vancouver.  
- The `GROUP BY` clause groups the records by `product_name` and computes the total sales for each product.  
- The `HAVING` clause filters the groups to include only those with total sales below $3000. It only applies on the groups.

---

Aggregates like `SUM`, `AVG`, or `COUNT` cannot be used in the `WHERE` clause because `WHERE` is processed before any grouping or aggregation occurs.

For example, the following query is **incorrect**:

```sql
SELECT product_name, SUM(sales_amount) AS total_sales
FROM sales
WHERE city = 'Vancouver' AND SUM(sales_amount) < 3000
GROUP BY product_name;
```

---

### Alternative Query: Filtering in `HAVING`

If you need to include conditions on both the city and aggregate values in the `HAVING` clause, you must include `city` in the `GROUP BY` clause.

Here’s the corrected query:

```sql
SELECT product_name, city, SUM(sales_amount) AS total_sales
FROM sales
GROUP BY product_name, city
HAVING SUM(sales_amount) < 3000 AND city = 'Vancouver';
```

- Adding `city` to the `GROUP BY` clause ensures that the grouping considers both `product_name` and `city`.  
- The `HAVING` clause filters groups based on total sales and the city.
