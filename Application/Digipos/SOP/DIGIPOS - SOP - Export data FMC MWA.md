---
tags:
  - sop
  - digipos
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/T6XtCw
---
#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

## Export Data All Sales

![[DIGPOS - Query#Export Data All Sales]]

## Daily

Sesuai request dari tim business per 23 Agustus 2025 export detail list tugas dengan status completed pada jam 7.

![[DIGPOS - Query#Export List Tugas For Daily]]