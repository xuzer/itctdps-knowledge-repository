---
tags:
  - sop
  - digipos
  - service-request
aplikasi:
  - DIGIPOS
link_sop: https://confluence.telkomsel.co.id/x/2prtCw
---
#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

## Workflow

![[Pasted image 20250826035727.png]]

## Pengerjaan

- Buka excel dari BU. Kolom “Target Sales Product Utama”, “Target Orbit” , “Target Cross Sell & Upsell”, “Target New SF”, “Min Ratio Healthy Sales”, “Target Healthy Sales” format cell ‘Number’; **tanpa separator dan tidak angka di belakang koma.**
- Pastikan di kolom “Target Sales Product Utama”, “Target Orbit” , “Target Cross Sell & Upsell”, “Target New SF”, “Min Ratio Healthy Sales”, “Target Healthy Sales” value nya **tidak dalam bentuk formula sebelum di-import.** Jika masih dalam bentuk formula, maka wajib di ubah dulu dengan cara “Copy => Paste value”
- Pastikan di kolom “Target Sales Product Utama”, “Target Orbit” , “Target Cross Sell & Upsell”, “Target New SF”, “Min Ratio Healthy Sales”, “Target Healthy Sales” **tidak ada separator koma (,) ataupun titik (.) sebelum di-import.**
- Kalau Target tidak mau diisi, silahkan diberi value `null`
- Pastikan untuk Seluruh variable column target merupakan **Number**

Lakukan import file tersebut ke database temp dengan user dgposrd, namanya dapat disesuaikan dengan waktu pengerjaan. Contoh: temp_kpi_agency_20250826.

Grant access table tersebut ke user DGPOS.

```sql
grant all on <nama_table> to dgpos;
```

Jika hasil import file tidak bisa berbentuk number, maka ubah query round.

1. Cara Pertama:  `ROUND(TO_NUMBER(TARGET_ORBIT))`
2. Cara Kedua: 
	1. `ROUND(replace(TARGET_ORBIT, '.', ','))` -- jika titik tidak bisa
	2. `ROUND(replace(TARGET_ORBIT, ',', '.'))` -- jika koma tidak bisa

Jalankan query berikut dan jika gagal silahkan lihat query di atas:

```
BU request update MWA Target KPI January 2025 => set period = January 2025 (H-0 month)
```

![[DIGPOS - Query#Insert To Temp Target KPI Agency]]

Setelah sukses jalankan procedure berikut:

```sql
BEGIN
    FMC_INSERT_TARGET_REKON;
END;
```