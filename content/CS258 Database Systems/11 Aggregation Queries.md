Many analytics queries are for computing **aggregation functions**
- e.g. `SUM, AVG, COUNT, MIN, MAX`
These can be applied to a column in a `SELECT` clause

`COUNT` counts the **number** of tuples
`COUNT DISTINCT` counts the number of tuples **after eliminating duplicate values**
#### Aggregate functions
**COUNT**(`<attName>`) - Number of rows `<attName>`
**SUM**(`<attName>`) - Sum values of attribute `<attName>`
**MAX**(`<attName>`) - Maximum value across tuples for `<attName>`
**MIN**(`<attName>`) - Minimum value across tuples for `<attName>` 
**AVG**(`<attName>`) - Average value across tuples for `<attName>`

![[Pasted image 20231023133035.png]]

![[Pasted image 20231023133213.png]]

#### Keyword `GROUP BY`
```sql
SELECT <attName>, func(<attName>), ...
FROM <tableList>
GROUP BY <attNameGroup>
```
- **Applies** aggregate functions across **groups** of rows rather than across **entire** query 
- Uses the attribute to group rows where the values match between the rows 
- `NULL` counts as a distinct group

![[Pasted image 20231023134211.png]]

![[Pasted image 20231023135346.png]]

![[Pasted image 20231023135444.png]]

- This would return all bars in one column and the same minimum price (minimum price of all) as the value of the MIN(price) column

#### `HAVING` clause
**Filters** groups
`HAVING <condition>` may follow a `GROUP BY` clause
If so, the condition applies to each group
- Groups not satisfying the condition are eliminated from the result

![[Pasted image 20231023135750.png]]
