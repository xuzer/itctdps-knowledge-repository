---
tags:
  - sop
  - official-sop
aplikasi:
  - DIGIPOS
link_sop:
---
### Catatan
#### Transaksi Paket
Dapat menggunakan query tersebut, dan dikirim ke SITTA
![[DIGPOS - Query#Cek Digistar Transaksi Paket]]
#### Transaksi PSB Indihome
#### Transaksi VF
#### Transaksi AP/ARP

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

