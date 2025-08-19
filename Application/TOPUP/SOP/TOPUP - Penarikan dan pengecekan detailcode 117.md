---
tags:
  - sop
  - official-sop
applications:
  - TOPUP
---
**URL:**
**Subject Email Report:** Diskusi Case Topup Corp 27 Feb 2024
Contoh body email: 
```
Berikut terlampir data transaksi pada pilot dengan BC01 bulan JULI yang terkena error 117 dari TopUp Corporate dan MEPRO
```


![[TopUp - QUERY#Query Provide Data Transaksi 117]]

#### News & Update
```dataview
TABLE file.name AS "News Title", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```
