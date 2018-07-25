# ex--oracle
Oracle Tips

____
# DESC
DESC _table_
```sql
DESC system.help
```

____
# ROWNUM
TOP 10
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
# ROWNUMBER()
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

____
# DATE
DATE,
TIMESTAMP _[WITH TIMEZONE]_
```sql
SELECT TO_DATE('2014/07/22', 'yyyy/mm/dd') FROM dual;
SELECT TO_CHAR(sysdate, 'YYYY') FROM dual;

INSERT INTO table(..., dt, ...) VALUES(..., CURRENT_TIMESTAMP, ...);
```
[Oracle / PLSQL TO_DATE](http://oracleplsql.ru/to_date-function.html)

____
# TIME_ZONE
```sql
ALTER SESSION SET TIME_ZONE = {}
```
{}:
- **local**
- **'+05:00'**
- **'Asia/Yekaterinburg'**

____
# FOR UPDATE
```sql
SELECT ... FOR UPDATE {}
```
{}:
- **wait 5** -- wait 5 sec, then fail
- **of(_columns_)** -- lock _columns_ only

____
# SEQUENCE
```sql
CREATE SEQUENCE sequence_name {}
SELECT sequence_name.nextval FROM dual;
```
{}:
- **MINVALUE _value_**
- **MAXVALUE _value_**
- **START WITH _value_**
- **INCREMENT BY _value_**
- **CYCLE | NOCYCLE**
- **CACHE | NOCACHE**
- **ORDER | NOORDER** ... guarantee that sequence numbers are generated in order of request.
This clause is useful if you are using the sequence numbers as timestamps.
Guaranteeing order is usually not important for sequences used to generate primary keys.

____
# INSERT ... (with sequence)
```sql
INSERT INTO table(id, ...) VALUES(sequence_name.nextval, ...);
```

____
# CREATE USER
```sql
CREATE USER uuu IDENTIFIED BY password;
GRANT CREATE SESSION, ... [CREATE TABLE, CREATE SEQUENCE] TO uuu;
ALTER USER uuu QUOTA UNLIMITED ON users QUOTA 100M ON test_ts QUOTA 500K ON data_ts;
```


____
# GRANT
```sql
CREATE ROLE rrr;
GRANT rrr TO uuu;
GRANT SELECT, INSERT, UPDATE, DELETE ON tableUUU TO uuu;
GRANT UPDATE {} ON tableRRR TO rrr;
```
{}:
- **_columns_** ... allow update _columns_ only

SELECT is NOT restricted (use VIEW to restrict)

```sql
REVOKE ALL ON tableUUU FROM uuu;
```
