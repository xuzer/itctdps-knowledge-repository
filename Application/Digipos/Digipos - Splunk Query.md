### Query Splunk DOM For Recon

```Splunk
index=app_backdom sourcetype=tselbackdom | eval createdAtM=strftime(_time, "%Y-%m-%d %H:%M:%S") | where channel_transaction_id in ("DGPS6USZQ7278I8CSPJP2Z26A","DGPSE7FIHXJIMAO52QYKXKOTU") | rename createdAtM as "created_at" | rex "DIGICORE_TDR\|(?<status>.*?)\|(?<statusDesc>.*?);tdr" | table created_at,channel_transaction_id,transaction_id,msisdn_a,msisdn_b,status,statusDesc
```