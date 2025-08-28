#### Query Provide Data Transaksi 117
```sql
SELECT
    *
FROM
    Transaction_log
WHERE
    TO_DATE (execution_time, '%Y-%m-%d') BETWEEN '2025-08-06' AND '2025-09-5'
```

#### Ambil Transaksi Hari Ini
```sql
SELECT * 
FROM Transaction_log 
WHERE execution_date >= CURDATE() 
  AND execution_date < CURDATE() + INTERVAL 1 DAY;
```

#### Summary Detail Code
```sql
SELECT DetailCode, count(*)
FROM Transaction_log 
WHERE execution_date >= CURDATE() 
  AND execution_date < CURDATE() + INTERVAL 1 DAY
GROUP BY DetailCode
;
```