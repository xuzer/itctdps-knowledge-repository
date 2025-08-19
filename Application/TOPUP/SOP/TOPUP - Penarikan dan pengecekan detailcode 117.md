---
tags:
  - sop
  - official-sop
applications:
  - TOPUP
---
**URL:**
**Notes:**
#### News & Update
```dataview
TABLE file.name AS "News Title", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```
