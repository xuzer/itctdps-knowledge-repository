#### Query Provide Data Transaksi 117
```sql
SELECT
    *
FROM
    Transaction_log
WHERE
    TO_DATE (execution_time, '%Y-%m-%d') BETWEEN '2025-08-06' AND '2025-09-5'
```

