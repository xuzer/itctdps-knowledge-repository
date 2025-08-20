---
tags:
  - sop
  - agent-sop
aplikasi:
  - DIGIPOS
status: Selesai
---
**URL:** -

### Notes
Dapat di-resolved dengan melampirkan kode voucher pada kolom E_VOUCHER_HRN. Dapat menggunakan query berikut 

![[DIGPOS - Query#Cek Transaksi E-Voucher]]

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

