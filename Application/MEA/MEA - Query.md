##### Update Failed RPP CHO

```sql
UPDATE change_ownership SET change_price_plan_status = 'CA' WHERE order_id = '' AND (change_price_plan_status = 'QUEUE' OR change_price_plan_status IS NULL);
```
##### Update Retry RPP CHO

```sql
update change_ownership set change_price_plan_status = 'CE' where  change_price_plan_status = 'QUEUE' and  order_id in (''); 
```
##### Get Change Price Plan Status

```sql
select id, order_id, msisdn, change_price_plan_status, change_price_plan_trx_id, change_price_plan_status_detail from change_ownership where order_id = '';
```
##### Update Full Product Status In Change Ownership Order TO CE
```sql
update change_ownership
set
    full_product_status = 'CE'
where
    1 = 1
    and order_id = 'CHO20250825041051704'
    and full_product_status = 'QUEUE';
```
##### Get Full Product Status In Change Ownership Order
```sql
select
    id,
    order_id,
    msisdn,
    full_product_status,
    full_product_trx_id,
    full_product_status_detail
from
    change_ownership
where
    order_id = 'CHO20250825041051704';
```
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
-- Update Sukses/Completed
UPDATE ordering SET total_success=total_request, status = 'Completed' WHERE id = '';
-- Update Failed
UPDATE ordering SET total_failed=total_request, status = 'Failed' WHERE id = '';
```
##### Cek Booked Payment
```sql
select * from booked_payment where payment_code = 'BP0002240000857';
```
##### Cek History Booked Payment
```sql
SELECT * FROM automatic_execute_payment_history WHERE book_payment_id in ('BP0002240000857');
```
##### Retry Booked Payment
```sql
update booked_payment set status = 'In Progress', is_retryable_execute_payment = 1, is_success_execute_payment = 0 where payment_code='BP000XXXXXXX';
```
