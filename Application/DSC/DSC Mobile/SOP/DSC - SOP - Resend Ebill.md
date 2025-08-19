---
tags:
  - sop
  - agent-sop
applications:
  - DSC
---
**URL:**

### Notes

![[DSC - General Knowledge#Ketentuan Resend Ebill]]


#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

