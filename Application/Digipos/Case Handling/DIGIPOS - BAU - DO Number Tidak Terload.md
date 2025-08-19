---
tags:
  - case-handling
application:
  - DIGIPOS
related_sop:
  - "[[DIGIPOS - Reproses DO & DOD]]"
---
Cek DO  & DOD apakah sudah ter-load dan sesuai menggunakan query berikut 

![[DIGPOS - Query#QUERY COMPARE Qty Order DO vs DOD]]

Jika ditemukan ada yang tidak sesuai atau belum ter-load silahkan cari terlebih dahulu do tersebut di file menggunakan command 

```bash
find xxxx
```

Jika ditemukan silahkan di reproses menggunakan sop [[DIGIPOS - Reproses DO & DOD]]