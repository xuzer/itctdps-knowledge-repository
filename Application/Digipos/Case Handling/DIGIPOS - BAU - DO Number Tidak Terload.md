---
tags:
  - case-handling
  - digipos
application:
  - DIGIPOS
related_sop:
  - "[[DIGIPOS - SOP- Reproses DO & DOD]]"
---
Cek DO  & DOD apakah sudah ter-load dan sesuai menggunakan query berikut 

![[DIGPOS - Query#QUERY COMPARE Qty Order DO vs DOD]]

Jika ditemukan ada yang tidak sesuai atau belum ter-load silahkan cari terlebih dahulu do tersebut di file menggunakan command 

```bash
find /path/to/search -name "*.gz" -exec zgrep -l "search_string" {} +
```

Jika ditemukan silahkan di reproses menggunakan sop [[DIGIPOS - SOP- Reproses DO & DOD]]