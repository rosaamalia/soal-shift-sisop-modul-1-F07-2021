# Penjelasan dan Penyelesaian Soal Shift Sisop Modul 1
Kelompok F07
1. Timotius Wirawan 05111940000161
2. Hana Machmudah 05111940000072
3. Rosa Amalia 05111940000106

## No.1
Membuat laporan pesan log beserta keterangan lainnya dari file `syslog.log`
### (a)
Menampilkan informasi dari file log. Informasi yang dibutuhkan diantaranya: jenis pesan log (INFO/ERROR0, pesan log dan username
### (b)
Menampilkan semua pesan error dengan jumlah kemunculannya
### (c)
Menampilkan jumlah kemunculan log ERROR dan INFO untuk setiap *user*-nya
### (d)
Informasi yang didapat di poin **(b)** dituliskan ke dalam file `error_message.csv` dengan header **Error, Count** diurutkan berdasarkan *jumlah kemunculan pesan error terbanyak*
### (e)
Semua informasi yang didapatkan pada poin **(c)** dituliskan ke dalam file `user_statistic.csv` dengan header **Username,INFO,ERROR** diurutkan berdasarkan *username* secara *ascending*

## No.2
Mencari kesimpulan dari data penjualan `Laporan-TokoShiSop.tsv`
### (a)
Menampilkan ID dan profit percentage terbesar dengan formula:
```
profit percentage = (profit/cost price) x 100
```
dengan *cost price* didapatkan dari pengurangan Sales dengan Profit.
### (b)
Menampilkan nama *customer* pada transaksi tahun 2017 di Albuquerque
### (c)
Menampilkan segment customer dan jumlah transaksinya yang paling sedikit
### (d)
Menampilkan wilayah bagian (region) yang memiliki total keuntungan (profit) paling sedikit dan total keuntungan wilayah tersebut
### (e)
Membuat script yang menghasilkan file **hasil.txt** dengan format yang sudah ditentukan


## No.3
Mengunduh gambar dari website tertentu
### (a)
Mengunduh 23 gambar dari `https://loremflickr.com/320/240/kitten` dan menyimpan log-nya ke file `Foto.log`, kemudian menyimpannya dengan nama `Koleksi_XX`
### (b)
Menjalankan script dengan jadwal sehari sekali pada jam 8 malam untuk tanggal-tanggal tertentu setiap bulan, yaitu dari tanggal 1 tujuh hari sekali (1,8,...), serta dari tanggal 2 empat hari sekali(2,6,...). Kemudian dipindahkan ke folder dengan nama sesuai tanggal unduhannya
### (c)
Mengunduh gambar dari `https://loremflickr.com/320/240/bunny`, kemudian diunduh secara bergantian dengan sumber di poin **(a)**. Kemudian disimpan ke folder dengan nama yang sesuai dengan jenisnya ("Kucing_" atau "Kelinci_")
### (d)
Memindahkan seluruh folder ke zip dan menguncinya dengan password
### (e)
Membuat koleksi sebelumnya ter-**zip** di waktu: setiap hari kecuali sabtu dan minggu, dari jam 7 pagi sampai 6 sore. Selain di jadwal tersebut, file ter-**unzip**
