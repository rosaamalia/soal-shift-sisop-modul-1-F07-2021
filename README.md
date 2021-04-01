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

### (a) Menampilkan ID dan profit percentage terbesar
Menggunakan formula: `profit percentage = (profit/cost price) x 100` dengan *cost price* didapatkan dari pengurangan Sales dengan Profit.
```
{ pp=$21/($18-$21)*100; if (pp>=max) { max=pp; id=$1 } }
```
- `pp=$21/($18-$21)*100` deklarasi nilai profit percentage, akan berubah di sesuai baris input
  - `$21` kolom **Profit**
  - `$18` kolom **Sales**
- `id=$1` variabel id menyimpan id dari kolom **Row ID**

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
