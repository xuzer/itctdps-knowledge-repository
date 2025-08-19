---
tags:
  - sop
  - official-sop
aplikasi:
  - MEA
link_sop: https://confluence.telkomsel.co.id/x/xITwC
---
### Catatan
Terdapat dua cara untuk melakukan retry booked payment
1. Menggunakan script automate
2. Menggunakan cara manual

#### Automate Script
Untuk melakukan update menggunakan script memiliki kekurangan yaitu **tidak bisa digunakan secara batch**

Lokasi script: /home/apps/l2_utils/script/book-payment-mevo-retry
runner script: index.js

#### Manual Step
Gunakan query [[MEA - Query#Cek Booked Payment]]] dan pastikan BP nya ada. Jika data tidak ada bisa di FU ke L2 atau info-kan ke requestor bahwa datanya tidak ada.

Jika datanya ada dan statusnya Failed. Lakukan pengecekan history payment-nya dengan query [[MEA - Query#Cek HIstory Payment BP]]. Perhatikan pada kolom `transaction_message` pastikan pesannya adalah `ESB Error: 504 Gateway Time-out` untuk pesan lainnya dapat di-koordinasikan terlebih dahulu dengan L2 dan tim FInnet apakah dapat dilakukan retry.

Jika dipastikan bisa di-retry gunakan query [[MEA - Query#Retry Booked Payment]]. Silahkan cek secara berkala transaksi-nya hingga statusnya berubah menjadi sukses/failed. Jika failed dapat dicek kembali history payment-nya.

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

