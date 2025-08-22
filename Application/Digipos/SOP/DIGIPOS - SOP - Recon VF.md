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

#### Template 
![[Rekon_VF_INIT_Diproses_20250818.xlsx]]
