---
tags:
  - sop
  - official-sop
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/hLTtCw
status: Belum Dikerjakan
---
### Catatan
Gunakan query [[DIGPOS - Query#Cari KYC By RS]] untuk mencari KYC user by RS number, lalu update menggunakan query [[DIGPOS - Query#Rollback KYC]] untuk melakukan rollback KYC user tersebut. Beberapa hal yang perlu diperhatikan adalah:
* Pastikan payment method-nya sesuai
* RS sudah sesuai dengan yang di-request

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

