---
tags:
  - query
  - cug
---

##### Cek gagal Renewal
```sql
SELECT trx.* 
FROM CMR_OFFERING_ACT_TRX_MERCHANT trx 
WHERE  
    ((trx.msisdn IN (SELECT msisdn FROM CMR_MERCHANT_AR_QUEUE WHERE TRANSID_RENEW IS NULL AND TRUNC(created_date) = TRUNC(SYSDATE)) 
    OR trx.msisdn IN (SELECT msisdn FROM CMR_OFFERING_ACT_TRX_MERCHANT WHERE TRUNC(created_date) = TRUNC(SYSDATE) AND status <> '3')) 
    AND TRUNC(trx.exp_date) = TRUNC(SYSDATE)) 
    AND TRUNC(trx.created_date) != TRUNC(SYSDATE)
    AND trx.msisdn not in (select msisdn from CMR_API_PCA_LOG where response='NOK' and process_name='CUG_MERCHANT_CALLBACK' and transid like '%u7%' and log_date > Trunc(Sysdate))
    AND trx.msisdn not in (select msisdn from CMR_OFFERING_ACT_TRX_MERCHANT where status = '3' and TRUNC(created_date) = TRUNC(SYSDATE);
```
##### Update transaksi ke nonaktif