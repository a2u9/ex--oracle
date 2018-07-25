# ex--oracle
Oracle Tips

____
## ROWNUM (TOP 10)

```sql
SELECT * FROM
   (SELECT * FROM emp ORDER BY salary)
WHERE ROWNUM < 11;
```

**NEVER** use this:
```sql
SELECT * FROM emp WHERE ROWNUM > 10;
```
____
## ROWNUMBER()

```sql
SELECT * FROM (SELECT a.*, ROW_NUMBER () OVER (ORDER BY a.object_name) rn
              FROM dba_objects a)
WHERE rn < 5;
```
Elapsed: 00:00:0**0.07**
```sql
SELECT * FROM (SELECT a.*, ROWNUM rn
              FROM dba_objects a
              ORDER BY a.object_name)
WHERE rn < 5;
```
Elapsed: 00:00:0**1.00**
