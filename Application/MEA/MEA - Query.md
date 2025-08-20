##### Cek History Change Ownership
```sql
select id, transaction_id, action, transaction_message, transaction_date from change_ownership_history where change_ownership_id in (select id from change_ownership where order_id = '');
```
##### Cek Ordering File
```sql
SELECT * FROM ordering_file 
WHERE order_id = 'CHO20250819180147929';
```
##### Update Ordering
```sql
UPDATE ordering SET total_success=total_request, status = 'Completed' WHERE id = '';
```
##### Cek Booked Payment
```sql
select * from booked_payment where payment_code = 'BP0002240000857';
```
##### Cek History Booked Payment
```sql
SELECT * FROM  automatic_execute_payment_history WHERE book_payment_id in ('BP0002240000857');
```
##### Retry Booked Payment
```sql
update booked_payment set status = 'In Progress', is_retryable_execute_payment = 1, is_success_execute_payment = 0 where payment_code='BP000XXXXXXX';
```
