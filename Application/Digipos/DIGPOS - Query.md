---
tags:
  - digipos
  - query
---

##### Open Menu DIGIPOS Per Outlet
```sql
INSERT INTO APP_MENU_OUTLET_WL SELECT '' AS MENU_ID, OUTLET_TYPE_ID,OUTLET_ID FROM OUTLET WHERE OUTLET_ID IN ('');
```
##### Provide data Recharge
###### Dealer code | Month | payment type (LinkAja/Finpay/NGRS) | package type (Regular/VAS) | trx | revenue
```sql
SELECT c.company_name, c.dealer_code, to_char(a.created_at, 'YYYY-MM') "MONTH",a.payment_method,a.type_trx, count(*) trx, sum(a.price) revenue FROM DGPOS.v_all_trx a
inner join (select user_id,company_id from "USER" where role_id = '204') b on a.created_by = b.user_id
inner JOIN (select distinct company_name, dealer_code, COMPANY_ID from DGPOS.COMPANY) C ON B.COMPANY_ID = C.COMPANY_ID 
WHERE a.created_at BETWEEN TO_DATE('20250401 00:00:00', 'YYYYMMDD HH24:MI:SS') AND TO_DATE('20250731 23:59:59', 'YYYYMMDD HH24:MI:SS')
and a.type_trx in ('Recharge Request','Package Activation')
and a.status in ('SUKSES','SUCCESS')
group by c.company_name, c.dealer_code, to_char(a.created_at, 'YYYY-MM'),a.payment_method,a.type_trx
order by to_char(a.created_at, 'YYYY-MM') , count(*) desc;
```
##### Insert Flagging Success RS
```sql
Insert into Reseller_Registration ( TRANSACTION_ID, REG_TYPE, OUTLET_ID, KTP, KTP_IMAGE, SELFIE_KTP_IMAGE, OUTLET_NAME, ADDRESS, CITY, POSTAL_CODE, NPWP, EMAIL, NO_HP_OWNER, NO_RS, CREATED_AT, CREATED_BY, UPDATED_AT, UPDATED_BY, KTP_NAME, ADDRESS_OUTLET, STATUS, STATUS_DESC, PAYMENT_METHOD, OPERATOR_STATUS, OPERATOR_STATUS_DESC, OPERATOR_REF_ID, PIN_STATUS, PIN_STATUS_DESC, PIN_REF_ID, ONBOARDING_TYPE, CHANNEL ) values ( 'DGPS240620151302536441210-FIN', 'ONBOARDING', '2501013635', '8120e90ikir93ff5acb6ea6e74e8fb9430d35435cb99ae0bb5a1fab8b051f89cef680', 'onboarding/rsIdCard.jpeg', 'onboarding/rsIdCard.jpeg', '6281295747771 - DHD CELL', 'adb68c6pjkc2088d7d909d694f8715820287c7764e044df074abe6480485d20bb6f1acddaed4cb6fe7bf3cee17a9469b35f754359589f7a0728ce130d48f371262f640df1b7fdd33f26bfd3df121b7d99718be5dc017a87253766858752fafdfd8d08', 'BOGOR', '42264', '7519463ky1rq99f21f97844bc7291b5837b01987d6ac9dc5d6cc8e073628134d805ad', '286d9978ws206b035c611b4ca92eff3f1d16200953b0cca2b1f76a268478ee79ca9fc', '81295747771', '81295747771', sysdate, NULL, sysdate, NULL, 'DHD CELL', 'Kp Sawah Barat DS Labuan RT/RW  003/012 kec. labuan Kab Pandeglang- Banten 42264', 'APPROVED', 'Process service request successfully.', 'FINPAY', 'SUCCESS', NULL, NULL, NULL, NULL, NULL, NULL, NULL );
```
##### Insert POI NPSN
```sql
INSERT INTO poi_npsn VALUES('10264164','MTSN KARO','0','KABANJAHE','KARO','PEMATANGSIANTAR','SUMBAGUT','SUMATERA','98.504732','3.104779');
```
##### Update POI NPSN
```sql
UPDATE POI_NPSN SET LATITUDE = '-2.935244’', LONGITUDE ='104.775820' WHERE npsn = '10603735';
```
##### Cek NPSN 
```sql
select * from POI_NPS where NPSN = ''
```
##### Rollback KYC
```sql
UPDATE reseller registration set status='FAILED' where outlet_id='1201119737' AND payment_method='FINPAY';
```
##### Cari KYC By RS
```sql
select * from reseller_registration where outlet_id in (select outlet_id from rs_outlet where rs_number='82235073136' and enabled='1');
```
##### Cek Digistar Transaksi Paket
```sql
SELECT * FROM dgpos.package_activaton 
WHERE 1=1
--AND msidn IN ()
AND ext_trx_id IN ()
ORDER BY created_at DESC;
```
##### Cek Transaksi E-Voucher
```sql
SELECT E_VOUCHER_ID,TRANSACTION_ID, RS_NUMBER,msisdn,PAYMENT_METHOD,STATUS, STATUS_DESC,CREATED_AT,E_VOUCHER_HRN, PRODUCT_ID, PRODUCT_NAME
FROM E_VOUCHER
WHERE MSISDN = '085102314573'
ORDER BY CREATED_AT DESC;
```
##### Suspend User Outlet
```sql
UPDATE outlet set enabled='0', updated_at=sysdate,updated_by='8888' where outlet_id='3201053384';
UPDATE rs_outlet set enabled='0', updated_at=sysdate,updated_by='8888' where outlet_id='3201053384';
UPDATE "USER" set enabled='0', is_activated='0', updated_at=sysdate,updated_by='8888' where user_id in (select owner_id from dgpos.outlet where outlet_id in ('3201053384') );
```
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