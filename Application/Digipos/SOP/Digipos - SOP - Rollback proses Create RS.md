---
tags:
  - sop
  - official-sop
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/hLTtCw
status: Selesai
---
### Catatan
Beberapa hal yang perlu diperhatikan adalah:
* Pastikan payment method-nya sesuai dengan yang di-request/complaint
* RS sudah sesuai dengan yang di-request

Untuk mencari KYC RS dapat menggunakan query
![[DIGPOS - Query#Cari KYC By RS]]

Jika ditemukan dan statusnya `INIT` dapat di update menggunakan query berikut
![[DIGPOS - Query#Rollback KYC]]
#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

