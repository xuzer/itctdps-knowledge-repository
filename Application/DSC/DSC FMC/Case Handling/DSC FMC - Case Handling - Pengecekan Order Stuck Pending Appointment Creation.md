---
tags:
  - case-handling
application:
  - DSC - FMC
related_sop:
---
Gunakan query untuk melakukan pengecekan slot notification

![[DSC - Query#Cek Slot Notifcation]]

Untuk melakukan penarikan data bisa menggunakan command berikut 

```sh
mysql -u dsc -h 10.59.102.140 -P 3340 -p'Bebas#2018' dsc 
--batch --raw 
-e "SELECT * FROM slot_notification WHERE order_id IN ('');" 
| sed 's/\t/|/g' > /tmp/cek_slot_pipe.csv
```

Cek pada bagian status. Perhatikan status dengan value **`pending`** atau **`waiting`** transaksi id nya bisa diambil dan lakukan pada pengecekan log submit appointment creation menggunakan query Splunk di bawah. *Step query di atas bersifat optional, semua order id bisa langsung digunakan untuk pengecekan di Splunk*

```Splunk
index="app_dsc_fmc_cloud"  sourcetype="tseldsc_fmc" AND "fmc-order" AND "submit-order/NM" NOT "RAW" serviceUri!=""
| rex field=_raw "\"order_id\"\s*:\s*\"(?<order_id>[^\"]+)\""
| rex field=_raw "\"appointment_status\"\s*:\s*\"(?<appointment_status>[^\"]+)\""
| rex field=_raw "\"appointment_start_date\"\s*:\s*\"(?<appointment_start_date>[^\"]+)\""
| eval match_order=if(order_id in ("AOi4250818104448185682d10",
"AOi42508181049066771f5920",
"AOi425081811092085835fd00",
"AOi4250818111246535b9dea0",
"AOi4250818113805747ac8200",
"AOi4250818115455192c2c320"), "yes", "no")
| search match_order="yes"
| table order_id, appointment_status, appointment_start_date
```

Jika sudah ada dan statusnya **`Avalilable`** berarti sudah dilakukan **appointment creation** 