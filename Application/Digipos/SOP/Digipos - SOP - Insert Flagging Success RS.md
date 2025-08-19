---
tags:
  - sop
  - official-sop
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/W7TtCw
---
### Catatan

Sebelum melakukan insert mohon pastikan bahwa ORG pada setiap payment method-nya sudah sukses

Gunakan query berikut untuk melakukan pengecekan apakah sudah ada di table `reseller_registration`

![[DIGPOS - Query#Cari KYC By RS]]

Jika sudah ada dapat dilakukan insert menggunakan query
![[DIGPOS - Query#Insert Flagging Success RS]]

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

