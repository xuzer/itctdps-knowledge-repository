---
tags:
  - sop
  - official-sop
  - dsc-mobile
aplikasi:
  - DSC
link_sop:
---
**URL:**

### Notes
#### Jika change ownership gagal karena nomor terbaca memiliki child, dengan indikasi pada system terbaca predictive bill
1. Lakukan Pisah Billing terlebih dahulu pada Child Number (pastikan bahwa Child Number BUKAN kategori Nomor Cantik (Golden Number) dan pastikan sudah tidak memiliki tunggakan tagihan/outstanding))
	1.  Jika child number diawali 62811, maka:
		- Lakukan Terminate
		-  Jika Terminate sudah berhasil maka status child number akan berubah menjadi AGING
		- Eskalasi ke BESHQ untuk permintaan perubahan status AGING menjadi AVAILABLE
		- Setelah status sudah berubah menjadi AVAILABLE maka lakukan proses RESERVED di CTP
		- Setelah nomor selesai di Reserved, kemudian dilanjutkan dengan proses Pasang Baru (PSB) oleh Grapari
	2.  Jika child number diawalai 62812, maka:
		-  Lakukan Terminate
		- Jika Terminate sudah berhasil maka status child number akan berubah menjadi AGING 
		- Eskalasi ke BESHQ untuk permintaan Migrasi Post to Pre
		- Setelah Migrasi Post to Pre berhasil, status nomor akan berubah Kembali menjadi Prabayar (Prepaid)
		- Setelah status nomor berubah menjadi Prabayar (Prepaid), kemudian dilanjutkan dengan proses Pasang baru (PSB) SPTP oleh Grapari
2. Setelah proses pisah billing antara Parent Number dan Child Number berhasil, maka kemudian dilanjutkan dengan proses Change Ownership pada nomor Parent Number sesuai permintaan Change Ownership tersebut.
3. Detail proses penanganan change ownership diatur dalam SOP-003/CCM-AC/BPM/II/2019 - Penanganan Balik Nama Pelanggan Telkomsel Halo melalui CTP - KM3416112234



#### News & Update
```dataview
TABLE file.name AS "News Title", tags AS "Categories", date AS "Date" FROM "News & Updates" WHERE contains(related_sop, this.file.link) SORT date DESC
```

