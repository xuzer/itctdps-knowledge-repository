---
tags:
  - sop
  - agent-sop
application:
  - DIGIPOS
---
**URL:**

#### Notes
* Untuk reproses DO & DOD memiliki unique constraint sehingga tidak akan mengalami double do.

**Reproses DO**
* file yang belum diproses harus ada di directory `/data/l2/deliveryorder/stock-parser`
* file yang sudah ter-proses nantinya ada di directory `/data/l2/deliveryorder/stock-parser/finished-files`
Run script reproses DO:
```bash
nohup /data/l2/deliveryorder/stock-parser/process_do.sh > /data/l2/deliveryorder/stock-parser/process_do.log &
```

---

**Reproses DOD** 
* file yang belum diproses harus ada di directory `/data/l2/deliveryorder`
* file yang sudah ter-proses nantinya ada di directory `/data/l2/deliveryorder/finished_insert_do_detail`
- ubah file isi `/data/l2/deliveryorder/insert_all.ctl` sesuai dengan file DO di tanggal yang ingin diproses.
- Semua file `getDO_Detail` dengan ekstensi .csv akan dipindahkan ke `finished_insert_do_detail` sehingga disarankan jika melakukan reproses lebih dari satu file satu - satu.
- Run Script reproses DOD
```sh
nohup /data/l2/deliveryorder/manually_insert_do.sh > /data/l2/deliveryorder/manually_insert_do.sh &
```

#### News & Updates
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```
