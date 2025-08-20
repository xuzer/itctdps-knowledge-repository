---
tags:
  - sop
  - official-sop
aplikasi:
  - MEA
link_sop:
status: Sedang Dikerjakan
---
### Catatan

#### Step sebelum Change Price Plan
Untuk order CHO jika belum masuk step Change Price Plan	, dapat menggunakan query berikut:

![[MEA - Query#Cek History Change Ownership]]

Jika mendapatkan error seperti berikut transaction_idnya dapat dicek di [[Splunk]]. Atau di server `omnidwpapp3` pada path `/var/log/web-api/rabbitmq`
![[Pasted image 20250820070328.png]]

Log yang didapatkan dapat di FU ke tim terkait servicenya.

#### Retry retry order CHO
Jika order pending atau ingin melakukan retry pada order CHO gunakan SOP [[MEA - SOP - Retry Order CHO]] 
#### Step Transfer Balance
#### Step Change Price Plan

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

