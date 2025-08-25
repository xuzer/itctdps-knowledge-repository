---
tags:
  - sop
  - MEA
aplikasi:
  - MEA
link_sop:
---
#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

### Pending Pada Order Change Ownership
1. Gunakan query berikut untuk mendapatkan status `full_product_status`-nya

![[MEA - Query#Get Full Product Status In Change Ownership Order]]

2. Jika statusnya `QUEUE` dapat di update ke `CE` lalu di retry melalui GUI.

![[MEA - Query#Update Full Product Status In Change Ownership Order]]