---
tags:
  - case-handling
  - dsc-fmc
  - dsc
application:
related_sop:
---
1. Lakukan pengecekan log yang mengarah ke `Docman` menggunakan query berikut
```splunk
https://fmc-document/fmc-api/document/v1/generate-contract
```
2. Jika pada `Docman` sudah sukses dan contract-nya sudah ada silahkan minta log dari UFO mengarah ke CRM. 
3. Jika dari CRM tidak ditemukan precontract IH number nya. Fu ke L2 DSC untuk dilakukan retry