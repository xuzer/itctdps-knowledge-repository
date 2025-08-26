---
tags:
  - digipos
  - query
---
##### Export Data All Sales
```sql
select
  psr.TRANSACTION_ID, psr.COMPANY_ID,psr.BRANCH_ID,psr.PERIOD,psr.TRANSACTION_DATE,psr.ORDER_TRANSACTION_ID,psr.USER_CODE,psr.NAME,psr.SUPERVISOR_CODE,psr.SUPERVISOR_NAME,psr.INTERNET_NUMBER,psr.APPS,  psr.STATUS,psr.CALLBACK_DATE,psr.WOK,psr.MARKETING_FEE_H2,psr.REPORT_CATEGORY,
  psrt.TYPE,psrt.PRICE,psrt.BUNDLE_NAME as PACKAGE_NAME,psrt.SALES_FEE_H2,psrt.PRODUCT_INDEX,
  bi.branch_index
from psb_sales_report psr
join psb_sales_report_type psrt on psr.transaction_id = psrt.transaction_id
JOIN branch_index bi on psr.branch_id = bi.branch_id
where psr.period = TO_DATE(:p_year || '-' || :p_month, 'yyyy-MM')
AND psr.REPORT_CATEGORY = 'SF';
```

```
BU request export list tugas status pending January 2025 => set period = January 2025 (H-0 month)
```

##### Insert To Temp Target KPI Agency
```sql
INSERT INTO TEMP_INSERT_TARGET_REKON(PERIOD, COMPANY_ID, BRANCH, WOK, USER_TYPE, TARGET_IH, TARGET_ARPU, TARGET_ORBIT, TARGET_CROSS_SELL, TARGET_NEW_SF, TARGET_SF,MIN_TARGET_HEALTHY_3,TARGET_HEALTHY_3, MIN_TARGET_HEALTHY_4, TARGET_HEALTHY_4,MIN_TARGET_HEALTHY_5, TARGET_HEALTHY_5)
SELECT
to_date('01-08-2025', 'dd-MM-yyyy') period, –- H-0 Month Request (jangan sampai salah period; H-0 bulan)** 
    company_id,                         -- id_wok
    branch,
    wok,
    'SF' user_type,                     --SF / TBG
    ROUND(TARGET_SALES_PRODUK_UTAMA),   --Target Sales
    null,                       --Target Average Price
    ROUND(TARGET_ORBIT),
    ROUND(TARGET_CROSS_SELL_UPSELL),
    ROUND(TARGET_NEW_SF_WITH_PS),--Target new SF Productivity
    null,               --Target exsiting SF Productivity
    ROUND(MIN_RATIO_HEALTHY_3,4),--Nilai Harus desimal dibawah 1
    ROUND(TARGET_RATIO_HEALTHY_3,4),--Nilai Harus desimal dibawah 1 ROUND(MIN_RATIO_HEALTHY_4,4), --Nilai Harus desimal dibawah 1
    ROUND(TARGET_RATIO_HEALTHY_4,4), --Nilai Harus desimal dibawah 1
    ROUND(MIN_RATIO_HEALTHY_5,4), --Nilai Harus desimal dibawah 1
    ROUND(TARGET_RATIO_HEALTHY_5,4)--Nilai Harus desimal dibawah 1
FROM table_baru;
```

##### Export List Tugas For Daily 
```sql
SELECT DISTINCT
  ps.SCHEDULE_ID AS "TUGAS ID", ps.NAME AS "TUGAS NAME", dj.JOURNEY_DATE AS "TUGAS DATE",
  tm.AREA_NAME AS "AREA", tm.REGIONAL AS "REGION", tm.BRANCH AS "BRANCH", dj.WOK AS "WOK",
  u.CODE AS "USER CODE", u.NAME AS "USER NAME", spv.CODE AS "SUPERVISOR CODE", spv.NAME AS "SUPERVISOR NAME",
  ps.SCHEDULE_CATEGORY AS "SCHEDULE CATEGORY", dj.STATUS AS "STATUS", dj.CHECK_IN AS "CHECKIN", dj.CHECK_OUT AS "CHECKOUT",
  ps.LATITUDE AS "LATITUDE", ps.LONGITUDE AS "LONGITUDE", ps.ODP AS "ODP",
  djff.ODP_CONDITION AS "ODP CONDITION", djff.ODP_CONDITION_DESC AS "ODP CONDITION DESC",
  djff.NUMBER_HOUSE_AROUND AS "NUMBER HOUSE AROUND", djff.PROSPECT_ESTIMATE AS "PROSPECT ESTIMATE",
  djff.FEEDBACK_LOCATION AS "FEEDBACK LOCATION", djff.NUMBER_COMPETITOR AS "NUMBER COMPETITOR",
  djff.FEEDBACK_COMPETITOR AS "FEEDBACK COMPETITOR", djff.NOTES AS "NOTES",
  (SELECT LISTAGG(b.COMPETITOR_DESC, ', ') WITHIN GROUP (ORDER BY b.COMPETITOR_DESC)
   FROM dgpos.DSI_JOURNEY_FMC_FB_COMP b WHERE b.JOURNEY_ID = djff.JOURNEY_ID) AS "COMPETITOR",
  (SELECT LISTAGG('[' || c.NAME || '-' || c.PHONE_NUMBER || '-' || c.ADDRESS || ']', ', ')
   WITHIN GROUP (ORDER BY c.NAME) FROM dgpos.DSI_JOURNEY_FMC_FB_PROSPECT c
   WHERE c.JOURNEY_ID = djff.JOURNEY_ID) AS "PROSPECT"
FROM dgpos.DSI_JOURNEY dj
JOIN dgpos.POI_SCHEDULE ps ON ps.SCHEDULE_ID = dj.SCHEDULE_ID
JOIN dgpos."USER" u ON u.USER_ID = dj.USER_ID
LEFT JOIN dgpos.USER_DSI ud ON ud.USER_ID = dj.USER_ID
LEFT JOIN dgpos."USER" spv ON spv.USER_ID = ud.SUPERVISOR_ID
LEFT JOIN dgpos.TERRITORY_MAPPING tm ON tm.BRANCH_ID = dj.BRANCH_ID
LEFT JOIN dgpos.DSI_JOURNEY_FMC_FEEDBACK djff ON djff.JOURNEY_ID = dj.JOURNEY_ID
WHERE dj.STATUS IN ('APPROVED', 'REJECTED') AND dj.JOURNEY_TYPE = 'POI'
  AND dj.JOURNEY_DATE BETWEEN TO_DATE('01-08-2025','DD-MM-YYYY') AND TO_DATE('25-08-2025','DD-MM-YYYY')
ORDER BY dj.JOURNEY_DATE DESC;
```
##### Summary Rekon
```sql
select
TO_CHAR(created_at, 'YYYY-MM-DD') AS CREATED_AT,TYPE_TRX,status,count(status) as jumlah_transaksi,sum(price) AS PRICE 
--count (price)
from(
    SELECT digicore_trx_id AS trx_id, status, created_at,UPDATED_BY,'RECHARGE' as TYPE_TRX,price,updated_at FROM DGPOS.recharge_request
    where created_at > trunc(sysdate-1)
    UNION ALL
    SELECT ext_trx_id AS trx_id, status, created_at,UPDATED_BY,'PHYSICAL_VOUCHER' as TYPE_TRX,price,updated_at FROM DGPOS.physical_voucher
    where created_at > trunc(sysdate-1)
    UNION ALL
    SELECT ext_trx_id AS trx_id, status, created_at,UPDATED_BY,'PACKAGE_ACTIVATION' as TYPE_TRX,price,updated_at FROM DGPOS.package_activation
    where created_at > trunc(sysdate-1)
    UNION ALL
    select distinct a.trx_id AS trx_id, a.status status, b.created_at created_at,a.UPDATED_BY UPDATED_BY, TRX_TYPE AS TYPE_TRX,price price,a.updated_at updated_at
    from dgpos.arp_request_detail A left join dgpos.arp_request B on A.REQUEST_ID=B.REQUEST_ID
    where a.created_at > trunc(sysdate-2)
)  where 
--updated_at > trunc(sysdate)  and 
--updated_at > sysdate -(8/24) 
--created_at BETWEEN to_date('20241231 00:00:00', 'rrrrmmdd hh24:mi:ss') and to_date ('20241231 23:59:59', 'rrrrmmdd hh24:mi:ss') 
updated_by='OP-SYS' 
group by TO_CHAR(created_at, 'YYYY-MM-DD'),status,TYPE_TRX
order by 1,2;
```
##### Get Recon VAS Already Success
```sql
select a.*,(now_time-trx_time)*24*60 res_time, 'Package Activation' TYPE_TRX, 'GAGAL' UPDATE_STATUS, 'Already Success' REASON from(
SELECT ACTIVATION_ID,OUTLET_ID,RS_NUMBER, MSISDN,STATUS,STATUS_DESC,CREATED_AT,EXT_TRX_ID,PRICE,PAYMENT_METHOD,to_date(TO_CHAR(CREATED_AT, 'YYYY/MM/DD HH24:MI:SS'),'YYYY/MM/DD HH24:MI:SS')trx_time 
,to_date(TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS'),'YYYY/MM/DD HH24:MI:SS')now_time
    FROM DGPOS.package_activation
        WHERE ext_trx_id IN (
                SELECT ext_trx_id
                    FROM dgpos.package_activation PA
                        WHERE ext_trx_id IN (
                                SELECT ext_trx_id FROM dgpos.package_activation
                                where STATUS in ('DIPROSES','INIT')
                                and created_at > trunc(sysdate -1) --and created_at < sysdate -(6/24) AND ext_trx_id = PA.ext_trx_id
                        )
                        AND STATUS in  ('SUKSES','SUCCESS')
        ) AND STATUS in ('DIPROSES','INIT')
ORDER BY CREATED_AT DESC) a;

```
##### Get TRX VAS INIT, DIPROSES
```sql
SELECT * FROM(
select a.*,(now_time-trx_time)*24*60 res_time, 'Package Activation' TYPE_TRX from(
select ACTIVATION_ID,OUTLET_ID,RS_NUMBER, MSISDN,STATUS,STATUS_DESC,CREATED_AT,EXT_TRX_ID,PRICE,PAYMENT_METHOD,to_date(TO_CHAR(CREATED_AT, 'YYYY/MM/DD HH24:MI:SS'),'YYYY/MM/DD HH24:MI:SS')trx_time 
,to_date(TO_CHAR(SYSDATE, 'YYYY/MM/DD HH24:MI:SS'),'YYYY/MM/DD HH24:MI:SS')now_time
from  DGPOS.PACKAGE_ACTIVATION
where created_at > trunc(sysdate -1)  --and created_at < sysdate -(6/24)
and status  in ('DIPROSES','INIT')
and status_desc is null
--and trx_type not like '%BYU%'
--and ext_trx_id not in ()
order by created_at desc
)a) WHERE res_time > 5;
```
##### Cek Summary Transksi Paket/VAS
```sql
select CR_DATE,SUM(TOTAL_VAS) TOTAL_VAS, SUM(TOTAL_VAS_SUKSES) SUKSES_VAS, SUM(TOTAL_VAS_GAGAL) GAGAL_VAS, SUM(TOTAL_VAS_INIT) VAS_INIT, SUM(TOTAL_VAS_DIPROSES) VAS_DIPROSES
FROM
(
select CR_DATE,
       CASE WHEN STATUS = 'SUKSES' and  trx_type not like '%BYU%'  THEN COUNT(STATUS) END TOTAL_VAS_SUKSES,
       CASE WHEN STATUS = 'GAGAL' and  trx_type not like '%BYU%'  THEN COUNT(STATUS) END TOTAL_VAS_GAGAL,
       CASE WHEN STATUS = 'INIT' and status_desc is null ---and  trx_type not like '%BYU%' 
       THEN COUNT(STATUS) END TOTAL_VAS_INIT,
       CASE WHEN STATUS = 'DIPROSES' and status_desc is null ---and trx_type not like '%BYU%' 
       THEN COUNT(STATUS) END TOTAL_VAS_DIPROSES,
       COUNT(STATUS) TOTAL_VAS
FROM
(
select to_char(CREATED_AT,'YYYY-MM-DD') CR_DATE, OUTLET_ID,MSISDN,STATUS,EXT_TRX_ID,status_desc,trx_type
from dgpos.Package_activation
where created_at > to_date('20250821 00:00:00','rrrrmmdd HH24:MI:SS') --and to_date('20231020 23:59:59','rrrrmmdd HH24:MI:SS')
)
group by CR_DATE,STATUS,status_desc,trx_type
)
group by CR_DATE
order by CR_DATE;
```
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