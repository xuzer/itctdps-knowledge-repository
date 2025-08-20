---
tags:
  - sop
aplikasi:
link_sop:
status: Selesai, Sedang Dikerjakan, Belum DIkerjakan
---
### Catatan



#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

