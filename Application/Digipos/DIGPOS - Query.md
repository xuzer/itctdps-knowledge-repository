
##### Insert to TEMP_INSERT_TARGET_REKON KPI Agency 
```sql
INSERT INTO TEMP_INSERT_TARGET_REKON(
  PERIOD, COMPANY_ID, BRANCH, WOK, USER_TYPE,
  TARGET_IH, TARGET_ARPU, TARGET_ORBIT, TARGET_CROSS_SELL,
  TARGET_NEW_SF, TARGET_SF,
  MIN_TARGET_HEALTHY_3, TARGET_HEALTHY_3,
  MIN_TARGET_HEALTHY_4, TARGET_HEALTHY_4,
  MIN_TARGET_HEALTHY_5, TARGET_HEALTHY_5
)
SELECT 
  TO_DATE('01-08-2025', 'dd-MM-yyyy') AS PERIOD,
  ID_WOK,
  BRANCH,
  WOK,
  'SF' AS USER_TYPE,
  ROUND(TO_NUMBER(REPLACE(TARGET_SALES_PRODUK_UTAMA, '%', ''))) AS TARGET_IH,
  NULL AS TARGET_ARPU,
  ROUND(TO_NUMBER(REPLACE(TARGET_ORBIT, '%', ''))) AS TARGET_ORBIT,
  ROUND(TO_NUMBER(REPLACE(TARGET_CROSS_SELL_UPSELL, '%', ''))) AS TARGET_CROSS_SELL,
  ROUND(TO_NUMBER(REPLACE(TARGET_NEW_SF_WITH_PS, '%', ''))) AS TARGET_NEW_SF,
  NULL AS TARGET_SF,
  ROUND(TO_NUMBER(REPLACE(REPLACE(MIN_RATIO_HEALTHY_3, '%', ''), ',', '.')) / 100, 4) AS MIN_TARGET_HEALTHY_3,
  ROUND(TO_NUMBER(REPLACE(REPLACE(TARGET_RATIO_HEALTHY_3, '%', ''), ',', '.')) / 100, 4) AS TARGET_HEALTHY_3,
  ROUND(TO_NUMBER(REPLACE(REPLACE(MIN_RATIO_HEALTHY_4, '%', ''), ',', '.')) / 100, 4) AS MIN_TARGET_HEALTHY_4,
  ROUND(TO_NUMBER(REPLACE(REPLACE(TARGET_RATIO_HEALTHY_4, '%', ''), ',', '.')) / 100, 4) AS TARGET_HEALTHY_4,
  ROUND(TO_NUMBER(REPLACE(REPLACE(MIN_RATIO_HEALTHY_5, '%', ''), ',', '.')) / 100, 4) AS MIN_TARGET_HEALTHY_5,
  ROUND(TO_NUMBER(REPLACE(REPLACE(TARGET_RATIO_HEALTHY_5, '%', ''), ',', '.')) / 100, 4) AS TARGET_HEALTHY_5
FROM temp_target_agency_20250819;
```
##### QUERY COMPARE Qty Order DO vs DOD
```sql
select do_number, 
    SUM(CASE WHEN status = 'DO' THEN qty ELSE 0 END) AS do_qty,
    SUM(CASE WHEN status = 'DOD' THEN qty ELSE 0 END) AS dod_qty,
	    item_code, block_from, block_to, company_code from
(SELECT 'DO' as status , do_number , sum(qty) qty , item_code, block_from, block_to, company_code
FROM DGPOS.DELIVERY_ORDER
group by do_number, item_code, block_from, block_to, company_code
UNION ALL
SELECT 'DOD' as status, do_number , count(*) qty, item_code, block_from, block_to, company_code
FROM DGPOS.DELIVERY_ORDER_DETAIL
group by do_number, item_code, block_from, block_to, company_code) A
WHERE A.do_number in (
'DO-322000RG-202508-0089',
'DO-322000RG-202508-0090'
)
GROUP BY do_number, item_code, block_from, block_to, company_code
order by do_number;
```
##### Cek Trend Transaksi VAS
```sql
SELECT 
    to_char (created_at , 'YYYY-MM-DD') as day,
    SUM(CASE WHEN UPPER(status) = 'SUKSES' THEN 1 ELSE 0 END) AS total_success,
    SUM(CASE WHEN UPPER(status) <> 'SUKSES' THEN 1 ELSE 0 END) AS total_gagal,
    COUNT(*) as total_trx,
    ROUND(
        (SUM(CASE WHEN UPPER(status) = 'SUKSES' THEN 1 ELSE 0 END) / COUNT(*)) * 100,
        2
    ) AS success_rate_persen
FROM package_activation
WHERE created_at BETWEEN 
      TO_TIMESTAMP('2025-08-01 00:00:00', 'YYYY-MM-DD HH24:MI:SS')
      AND TO_TIMESTAMP('2025-08-10 23:59:59', 'YYYY-MM-DD HH24:MI:SS')
GROUP BY to_char (created_at , 'YYYY-MM-DD')
ORDER BY to_char (created_at , 'YYYY-MM-DD') ASC;
``` 
##### Cek Trend Regular Recharge
```sql
SELECT 
    to_char (created_at , 'YYYY-MM-DD') as day,
    SUM(CASE WHEN UPPER(status) = 'SUKSES' THEN 1 ELSE 0 END) AS total_success,
    SUM(CASE WHEN UPPER(status) <> 'SUKSES' THEN 1 ELSE 0 END) AS total_gagal,
    COUNT(*) as total_trx,
    ROUND(
        (SUM(CASE WHEN UPPER(status) = 'SUKSES' THEN 1 ELSE 0 END) / COUNT(*)) * 100,
        2
    ) AS success_rate_persen
FROM recharge_request
WHERE created_at BETWEEN 
      TO_TIMESTAMP('2025-08-01 00:00:00', 'YYYY-MM-DD HH24:MI:SS')
      AND TO_TIMESTAMP('2025-08-10 23:59:59', 'YYYY-MM-DD HH24:MI:SS')
GROUP BY to_char (created_at , 'YYYY-MM-DD')
ORDER BY to_char (created_at , 'YYYY-MM-DD') ASC;
```
##### Cek Trend Transaksi ST
```sql
SELECT 
    to_char (created_at , 'YYYY-MM-DD') as day,
    SUM(CASE WHEN status IN ('COMPLETED SELLTHRU','COMPLETED','PARSIAL') THEN 1 ELSE 0 END) AS total_success,
    SUM(CASE WHEN status NOT IN ('COMPLETED SELLTHRU','COMPLETED','PARSIAL') THEN 1 ELSE 0 END) AS total_gagal,
    COUNT(*) as total_trx,
    ROUND(
        (SUM(CASE WHEN  status IN ('COMPLETED SELLTHRU','COMPLETED','PARSIAL') THEN 1 ELSE 0 END) / COUNT(*)) * 100,
        2
    ) AS success_rate_persen
FROM product_distribution
WHERE created_at BETWEEN 
      TO_TIMESTAMP('2025-08-01 00:00:00', 'YYYY-MM-DD HH24:MI:SS')
      AND TO_TIMESTAMP('2025-08-10 23:59:59', 'YYYY-MM-DD HH24:MI:SS')
GROUP BY to_char (created_at , 'YYYY-MM-DD')
ORDER BY to_char (created_at , 'YYYY-MM-DD') ASC;
```