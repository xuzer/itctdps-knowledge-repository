---
tags:
  - sop
  - official-sop
  - MyEnterprise
aplikasi:
  - MyEnterprise
link_sop: https://confluence.telkomsel.co.id/x/f4EhCg
---
### Catatan

Database Access: b2bchannelpapp1
Database Access Quota Pooling : b2bchannelpapp7

**QUERY**
![[MyEnterprise- Query#Report Weekly]]


#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

