# Penjelasan dan Penyelesaian Soal Shift Sisop Modul 1
Kelompok F07
1. Timotius Wirawan 05111940000161
2. Hana Machmudah 05111940000072
3. Rosa Amalia 05111940000106

## No.1
Membuat laporan pesan log beserta keterangan lainnya dari file *syslog.log*. Pada file tersebut berisikan data sebagai berikut:
```
...
Jan 31 00:44:34 ubuntu.local ticky: ERROR Permission denied while closing ticket (ac)
Jan 31 01:00:50 ubuntu.local ticky: INFO Commented on ticket [#4709] (blossom)
...
```
dengan setiap barisnya memiliki pola:
```
<time> <hostname> <app_name>: <log_type> <log_message> (<username>)
```

### (a)
Membuat regex untuk enampilkan informasi dari file log. Informasi yang dibutuhkan diantaranya: jenis pesan log (INFO/ERROR), pesan log dan username.
```
grep -oE "(INFO.*)|(ERROR.*)" syslog.log
```
- `grep` command untuk mencari file dengan pola yang telah ditentukan
- `-o` option untuk menampilkan bagian yang hanya sesuai dengan pola
- `-E` menerjemahkan pola sebagai extended regular expression (ERE)
- `"(INFO.*)|(ERROR.*)"` pola yang akan dicari
  - Tanda kurung `()` digunakan untuk mengelompokkan pola dan dapat mereferensikan mereka sebagai satu item
  - Simbol bintang `*` pada `INFO.*` dan `EEROR.*` adalah quantifier untuk mencocokkan pola dimulai dari nol dan seterusnya
  - Operator pipe `|` memiliki arti alternatif sebagai *"OR"*.  Operator ini memungkinkan untuk mencari kemungkinan pola dari `ERROR.*` dan `INFO.*`
 
### (b)
Menampilkan semua pesan error dengan jumlah kemunculannya.
### (c)
Menampilkan jumlah kemunculan log ERROR dan INFO untuk setiap *user*-nya
### (d)
Informasi yang didapat di poin **(b)** dituliskan ke dalam file `error_message.csv` dengan header **Error, Count** diurutkan berdasarkan *jumlah kemunculan pesan error terbanyak*
### (e)
Semua informasi yang didapatkan pada poin **(c)** dituliskan ke dalam file `user_statistic.csv` dengan header **Username,INFO,ERROR** diurutkan berdasarkan *username* secara *ascending*

## No.2
Mencari kesimpulan dari data penjualan `Laporan-TokoShiSop.tsv`
```
#!/usr/bin/bash

awk -F"\t" '
#soal a
BEGIN{ max=0; home=0; consumer=0; corporate=0; central=0; east=0; south=0; west=0 }
{ pp=$21/($18-$21)*100; if (pp>=max) { max=pp; id=$1 } }

#soal b
{ if ((match($3, "-17")) && $10=="Albuquerque") { nama[$7] } }

#soal c
{ if ($8=="Home Office") { home++ } else if ($8=="Consumer") { consumer++ } else if ($8=="Corporate") { corporate++ } }

#soal d
{ if ($13=="Central") { central+=$21 } else if ($13=="East") { east+=$21 } else if ($13=="South") { south+=$21 } else if ($13=="West") { west+=$21 } }

END {
#soal e
printf "Transaksi terakhir dengan profit percentage terbesar yaitu %d dengan presentase %d%%.\n\n", id, max
printf "Daftar nama customer di Albuquerque pada tahun 2017 antara lain:\n"

for (i in nama)
	printf "%s\n", i
  
if (home<=consumer && home<=corporate)
	printf "\nTipe segmen customer yang penjualannya paling sedikit adalah Home Office dengan %d transaksi.\n", home
else if (consumer<=home && consumer<=corporate)
	printf "\nTipe segmen customer yang penjualannya paling sedikit adalah Consumer dengan %d transaksi.\n", comsumer
else
	printf "\nTipe segmen customer yang penjualannya paling sedikit adalah Corporate dengan %d transaksi.\n", corporate
  
if (central<=east && central<=south && central<=west)
	printf "\nWilayah bagian (region) yang memiliki total keuntungan (profit) yang paling sedikit adalah Central dengan total keuntungan %f.\n", central
else if (east<=central && east<=south && east<=west)
	printf "\nWilayah bagian (region) yang memiliki total keuntungan (profit) yang paling sedikit adalah East dengan total keuntungan %f.\n", east
else if (south<=central && south<=east && south<=west)
	printf "\nWilayah bagian (region) yang memiliki total keuntungan (profit) yang paling sedikit adalah South dengan total keuntungan %f.\n", south
else
	printf "\nWilayah bagian (region) yang memiliki total keuntungan (profit) yang paling sedikit adalah West dengan total keuntungan %f.\n", west
}
' Laporan-TokoShiSop.tsv > hasil.txt
```

- `-F"\t"` untuk mengubah field separator menjadi tab per kolom
- `BEGIN{ max=0; home=0; consumer=0; corporate=0; central=0; east=0; south=0; west=0 }` mendeklarasikan variabel di blok BEGIN

### (a)
Menampilkan ID dan profit percentage terbesar menggunakan formula: `profit percentage = (profit/cost price) x 100` dengan *cost price* didapatkan dari pengurangan Sales dengan Profit.
```
{ pp=$21/($18-$21)*100; if (pp>=max) { max=pp; id=$1 } }
```
- `pp=$21/($18-$21)*100` deklarasi nilai profit percentage, akan berubah di sesuai baris input
  - `$21` kolom **Profit**
  - `$18` kolom **Sales**
- `id=$1` variabel id menyimpan id dari kolom **Row ID**

### (b)
Menampilkan nama *customer* pada transaksi tahun 2017 di Albuquerque
```
{ if ((match($3, "-17")) && $10=="Albuquerque") { nama[$7] } }
```
- `if ((match($3, "-17")) && $10=="Albuquerque")` mencari kondisi yang sesuai
  - `match($3, "-17"` menggunakan fungsi match() untuk mencari record yang mengandung **"-17"** (tahun 2017) di kolom `$3` **Order Date**
  - `$10=="Albuquerque"` mencari record yang persis sama dengan **"Albuquerque"** di kolom `$10` **City**
- `{ nama[$7] }` memasukkan nama di kolom `$7` **Customer Name** ke array nama

### (c)
Menampilkan segment customer dan jumlah transaksinya yang paling sedikit
```
{ if ($8=="Home Office") { home++ } else if ($8=="Consumer") { consumer++ } else if ($8=="Corporate") { corporate++ } }
```
- Mencari kondisi yang sesuai di kolom **Segment**
  - `$8` Kolom **Segment** 
- Setiap kondisi yang sesuai ditemukan, variabel untuk menghitung masing-masing segment akan bertambah

### (d)
Menampilkan wilayah bagian (region) yang memiliki total keuntungan (profit) paling sedikit dan total keuntungan wilayah tersebut
```
{ if ($13=="Central") { central+=$21 } else if ($13=="East") { east+=$21 } else if ($13=="South") { south+=$21 } else if ($13=="West") { west+=$21 } }
```
- Mencari kondisi yang sesuai di kolom **Region**
  - `$13` kolom **Region**
- Menambahkan profit dari kolom **Profit** ke variabel untuk menghitung jumlah profit masing-masing region
  - `$21` kolom **Profit**

### (e)
Membuat script yang menghasilkan file **hasil.txt** dengan format yang sudah ditentukan.
Pada blok END akan mencetak setiap soal yang telah dibuat dengan format yang sudah ditentukan.
```
Laporan-TokoShiSop.tsv > hasil.txt
```
- `Laporan-TokoShiSop.tsv` sumber file yang menjadi input
- `> hasil.txt` hasil output akan dikirim ke file **hasil.txt**

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
