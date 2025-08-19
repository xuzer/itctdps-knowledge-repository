##### Cek Ordering File
```sql
SELECT * FROM ordering_file 
WHERE order_id = 'CHO20250819180147929';
```
##### Update Ordering
```sql
UPDATE ordering SET total_success=total_request, status = 'Completed' WHERE id = '';
```


