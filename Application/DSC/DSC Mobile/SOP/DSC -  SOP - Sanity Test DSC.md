---
tags:
  - sop
  - official-sop
applications:
  - DSC
---
**URL:**

### Notes
**REPORT WAG: L1 L2 DSC Support**

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

