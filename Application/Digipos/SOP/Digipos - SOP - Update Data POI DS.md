---
tags:
  - sop
  - official-sop
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/7gDTD
---
### Catatan



#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

