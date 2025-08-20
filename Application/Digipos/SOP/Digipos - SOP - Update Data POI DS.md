---
tags:
  - sop
  - official-sop
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/7gDTD
status: Belum Dikerjakan
---
### Catatan

Sheet Request: https://docs.google.com/spreadsheets/d/1vmLgE-9XTU_ewoW8RbZgnTazGnnmJhaiElpdiHvewqE/edit?gid=0#gid=0

Lakukan validasi koordinat terlebih dahulu menggunakan program pada projek [[Projek - Cek Poi]]. Jika data koordinat-nya valid remark pada sheet.

Lakukan pengecekan apakan NPSN tersebut sudah ada menggunakan query [[DIGPOS - Query#Cek NPSN]].  Jika NPSN sudah tersedia gunakan query [[DIGPOS - Query#Update POI NPSN]], jika belum tersedia gunakan query [[DIGPOS - Query#Insert POI NPSN]]

#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

