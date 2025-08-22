---
tags:
  - sop
  - digipos
  - official-sop
aplikasi:
  - DIGIPOS
link_sop:
---
#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

### Template 

![[REKON_VAS_20250821.xlsx]]

### Cek Summary Recon VAS

![[DIGPOS - Query#Cek Summary Transksi Paket/VAS]]

### Get Status INIT, DIPROSES

![[DIGPOS - Query#Get TRX VAS INIT, DIPROSES]]

### Reporting Recon
![[DIGPOS - Query#Summary Rekon]]