---
tags:
  - sop
aplikasi:
  - MEA
link_sop:
status: Selesai
---
### Catatan

* Gunakan browser yang support fitur Edit and Resend, Browser berbasis Firefox/Edge memiliki fitur ini

**API:** https://api-dsc.telkomsel.com/myenterprise-access/change-ownership/retry
**method:** POST
**body:** `{"orderId":"order_id_CHO","activityName":"Validate MSISDN"}  `

Untuk body, pastikan order id sudah diganti menggunakan order yang ingin di-retry. Activity name disesuaikan dengan step yang ingin di retry.

```
Validate MSISDN
Mass Create Customer
Mass Create Contact
Mass Create Billing Profile
```

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

