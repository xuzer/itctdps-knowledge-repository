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

Jika terdapat log failed dapat di FU ke L2

#### Retry retry order CHO
Jika order pending atau ingin melakukan retry pada order CHO gunakan SOP [[MEA - SOP - Retry Order CHO]] 
#### Step Transfer Balance
#### Step Change Price Plan

Gunakan query berikut untuk mendapatkan status `change_price_plan_status`

![[MEA - Query#Get Change Price Plan Status]]

Jika statusnya `QUEUE` pastikan tidak ada order `RPP` pada `DSC` para periode setelah order CHO. 

![[Pasted image 20250825073738.png|Terdapat Order RPP Setelah Periode CHO dan Bukan Dari Channel MS]]

Jika sudah ada bisa di update ke Failed dengan query berikut

![[MEA - Query#Update Failed RPP CHO]]

Jika belum ada order RPP bisa di update ke CE lalu di retry via GUI dengan query berikut.

![[MEA - Query#Update Retry RPP CHO]]

Lakukan update ordering setelah update di atas:

![[MEA - Query#Update Ordering]]
#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

