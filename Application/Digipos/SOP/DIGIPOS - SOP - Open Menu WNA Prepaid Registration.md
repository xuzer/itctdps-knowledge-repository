---
tags:
  - sop
  - digipos
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/DoNQCw
---
### Catatan

MENU ID
1. DGP-OLT-WNA
2. DGP-OLT-WNA-AUTO-APPROVE (Untuk Auto Approve)

Dapat open dengan melakukan insert ke table whitelist

![[DIGPOS - Query#Open Menu DIGIPOS Per Outlet]]


#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

