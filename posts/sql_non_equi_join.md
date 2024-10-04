---
title: '[SQL] Things that you may not know about Non-equi join in SQL'
date: '2024-10-02'
---

If you think tables can be joined only using the common field, then this blog is for you.  
Besides equi-join, there is another way of joining called non-equi join, which is also commonly used in SQL queries.

**Let's first have a look at an example that uses equi-join:**

There are two relations:  
The **Orders** relation has the following attributes: 
- order_id(PK) 
- custome_name
- order_date  

The **Deliveries** relation has the following attributes: 
- delivery_id(PK) 
- order_id(FK)
- delivery_date

Let's select the `order_id`, `order_date`, and the `delivery_date` of all the orders:

```sql
SELECT O.order_id, O.order_date, D.delivery_date
FROM Orders O INNER JOIN Deliveries D ON O.order_id=D.order_id;
```


Easy, right?

Let's change the question a bit. How about selecting orders that are delivered 14 days after the `order_date` (late delivery)?

```sql
SELECT O.order_id, O.order_date, D.delivery_date
FROM Orders O INNER JOIN Deliveries D ON O.order_id=D.order_id
WHERE DATEDIFF(D.delivery_date, O.order_date) > 14;
```

```sql
SELECT O.order_id, O.order_date, D.delivery_date
FROM Orders O INNER JOIN Deliveries D ON O.order_id=D.order_id AND DATEDIFF(D.delivery_date, O.order_date) > 14;
```

For inner joins, placing the inequality in the `ON` clause or the `WHERE` clause will typically result in the same output. However, the placement can affect performance depending on the database and the size of the dataset.

## Use cases for non-equi joins:

1. **List all unique pair combinations:**  
The following query selects all unique pairs of renters who want to live in the same district:

```sql
SELECT r1.name, r1.preferred_district, r2.name, r2.preferred_district
FROM renters r1
JOIN renters r2
ON r1.preferred_district = r2.preferred_district AND r1.id < r2.id;
```  
*Note: We shouldn't use `r1.id != r2.id` because (renter1, renter2) and (renter2, renter1) are duplicated.*

2. **Joining tables using a range of values:**  
The following query finds renters matching available residences based on the range of rent and the number of bedrooms.

```sql
SELECT r.id, r.name, h.id, h.address, h.rent, h.bedrooms
FROM renters r
JOIN houses h
ON h.district = r.preferred_district
    AND h.rent BETWEEN r.min_rent AND r.max_rent
    AND h.bedrooms >= r.min_bedrooms
WHERE h.id NOT IN (SELECT house_id FROM deals);
```

For inner joins, placing the inequality in the `ON` clause or the `WHERE` clause will typically result in the same output. However, the placement can affect performance depending on the database and the size of the dataset. For outer joins, the behavior can differ, as the `ON` clause influences which rows are included in the result set before filtering.

## Example of how placement affects LEFT JOIN

In a `LEFT JOIN`, the placement of conditions can impact the results. For instance, consider the following query that lists all houses along with the date of any corresponding deals if they occurred after March 1st:

```sql
SELECT h.id, h.address, d.date
FROM houses h
LEFT JOIN deals d
ON h.id = d.house_id
WHERE d.date >= '2020-03-01';  -- This filters out houses without deals
```

The result will include only the houses with deals after March 1st, as the `WHERE` clause effectively cancels the `LEFT JOIN`, resulting in an `INNER JOIN`.

To keep all houses in the results, even those without matching deals, we can move the condition to the `ON` clause:

```sql
SELECT h.id, h.address, d.date
FROM houses h
LEFT JOIN deals d
ON h.id = d.house_id AND d.date >= '2020-03-01';  -- Non-equi JOIN
```

Now, all houses are listed, regardless of whether they have a corresponding deal.

## Conclusion

Non-equi joins provide powerful ways to combine data based on conditions beyond simple equality, enabling complex queries that can yield meaningful insights.

