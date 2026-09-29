# Soal Unguided — Penerapan OOP Sederhana untuk Pengolahan Data




## Petunjuk Umum

Buatlah program Java berdasarkan cerita berikut. Rancang sendiri struktur program yang diperlukan sebelum menuliskan kode.

- Tentukan sendiri field, constructor, dan method yang diperlukan, mengikuti hint yang disediakan.
- Data di dalam object tidak boleh diakses atau diubah langsung dari luar class-nya.
- Gunakan nama parameter yang sama dengan nama field pada constructor (sehingga `this` perlu dipakai secara eksplisit).
- Program ini harus memuat dua jenis field: **instance field** (milik masing-masing object) dan **class field** (milik class, dipakai bersama).
- Tidak perlu inheritance, overriding, maupun abstract class.

---

## Soal — Pembersihan dan Analisis Data Suhu Mingguan

Sebuah tim mengumpulkan suhu harian suatu kota selama satu minggu (7 hari) dari sensor otomatis. Data ini sudah tersedia sebagai array di `main` sebelum diserahkan untuk diolah.

Pada salah satu hari, sensor gagal mengirim data. Hari yang gagal terekam ditandai dengan sebuah nilai penanda pada array, karena suhu asli tidak mungkin sebesar itu. Nilai penanda ini sama untuk semua data suhu yang diolah kapan pun dan oleh siapa pun — bukan sesuatu yang berbeda-beda tiap kali data dibuat — sehingga sebaiknya ditulis satu kali saja sebagai aturan tetap milik class, bukan diketik ulang sebagai angka `-1.0` di setiap method yang membutuhkannya.

Buatlah satu class yang bertugas mengolah data suhu ini — memeriksa apakah ada data kosong, mengisinya, dan menghasilkan statistik dasar dari data yang sudah bersih.

### Kebutuhan program

Class ini harus dapat:

1. **Menyimpan data suhu 7 hari**, dibuat dari array suhu mentah yang sudah ada di `main` (index array mewakili hari).
2. **Menyimpan nilai penanda data kosong (`-1.0`) sebagai aturan tetap milik class**, yaitu class field yang nilainya tidak dapat diubah dan dipakai bersama oleh semua object.
3. **Menampilkan seluruh data apa adanya**, menandai hari yang kosong secara jelas.
4. **Mencari apakah ada hari yang kosong, dan di index berapa**, karena index inilah yang nanti dipakai untuk proses pengisian data.
5. **Mengisi hari yang kosong**, menggunakan index dari poin 4, dengan nilai perkiraan berdasarkan rata-rata suhu satu hari sebelum dan satu hari sesudahnya.
6. **Menghitung rata-rata suhu** dari data yang sudah bersih.

### Hint

> Hint di bawah ini adalah acuan minimal. Anda tetap boleh menambah method atau menyesuaikan detail selama seluruh kebutuhan di atas terpenuhi.

**Nama class:** `PengolahSuhu`

**Field yang disarankan:**

| Field | Jenis | Tipe | Keterangan |
|---|---|---|---|
| `suhuHarian` | Instance field | `double[]` | Menyimpan array suhu 7 hari; berbeda untuk setiap object |
| `NILAI_KOSONG` | Class field | `double` | Nilai penanda data kosong (`-1.0`); sama untuk semua object dan tidak berubah |

**Method yang disarankan:**

| Method | Kebutuhan | Return |
|---|---|---|
| `tampilkanData()` | 3 | `void` |
| `cariIndexKosong()` | 4 | `int` — index hari yang bernilai `NILAI_KOSONG`, atau `-1` apabila tidak ditemukan |
| `isiDataKosong()` | 5 | `void` — memanggil `cariIndexKosong()` di dalamnya; jika hasilnya bukan `-1`, isi index tersebut |
| `hitungRataRata()` | 6 | `double` |

**Rumus imputasi (pengisian data kosong) untuk time series:**

Untuk index hari `i` yang kosong:

```
suhuHarian[i] = ( suhuHarian[i-1] + suhuHarian[i+1] ) / 2
```

Ini adalah metode imputasi paling dasar untuk data deret waktu, yaitu **mengisi nilai yang hilang dengan rata-rata dari titik data tetangga terdekatnya** (sebelum dan sesudah), dengan asumsi nilai yang hilang berada di antara dua kondisi yang sudah diketahui dan tidak berubah drastis.

### Data suhu (array di `main`)

Gunakan array berikut sebagai data suhu 7 hari (Hari 1 = index 0):

```java
double[] suhuHarian = { 30.4, 24.3, 26.8, -1.0, 31.4, 30.8, 32.9 };
```

Hari yang kosong: **Hari 4**.

### Alur program di `main`

1. Siapkan array `suhuHarian` seperti di atas.
2. Buat object `PengolahSuhu` dari array `suhuHarian` tersebut. Tampilkan data awal (`tampilkanData()`), lalu panggil `cariIndexKosong()` dan tampilkan index yang ditemukan.
3. Jalankan `isiDataKosong()`.
4. Tampilkan data akhir yang sudah bersih (`tampilkanData()`) beserta rata-rata suhunya.
5. Tampilkan kembali isi array `suhuHarian` di `main` (bukan lewat object `PengolahSuhu`, melainkan variabel array yang sudah disiapkan di langkah 1). Amati bahwa array ini pun sudah berubah, padahal tidak pernah diubah langsung di `main`, lalu jelaskan sebabnya dalam beberapa kalimat sebagai komentar pada kode program.

### Output yang diharapkan

```
=== Data Suhu Awal ===
Hari 1 : 30.4°C
Hari 2 : 24.3°C
Hari 3 : 26.8°C
Hari 4 : (kosong)
Hari 5 : 31.4°C
Hari 6 : 30.8°C
Hari 7 : 32.9°C

Index hari kosong (dimulai dari 0): 3

=== Data Suhu Setelah Pengisian ===
Hari 1 : 30.4°C
Hari 2 : 24.3°C
Hari 3 : 26.8°C
Hari 4 : 29.1°C
Hari 5 : 31.4°C
Hari 6 : 30.8°C
Hari 7 : 32.9°C

Rata-rata : 29.39°C

Isi array suhuHarian di main setelah isiDataKosong() dijalankan:
[30.4, 24.3, 26.8, 29.1, 31.4, 30.8, 32.9]
(ikut berubah: constructor menyimpan referensi array yang sama)
```

---

## Yang perlu diperhatikan saat mengerjakan

- Amati baik-baik dua baris terakhir pada output di atas. Array `suhuHarian` yang dicetak itu adalah variabel di `main`, bukan diambil lewat object `PengolahSuhu` — dan nilainya sudah berubah, padahal `main` tidak pernah memanggil operasi pengubahan itu sendiri. Ini terjadi karena array bertipe **reference**: constructor `PengolahSuhu` menyimpan array yang sama persis (bukan menyalinnya), sehingga perubahan yang dilakukan `isiDataKosong()` pada object otomatis terlihat juga dari sisi `main`. Jelaskan hal ini dalam beberapa kalimat sebagai komentar pada kode program.
- Nilai penanda `-1.0` harus ditulis satu kali saja sebagai class field, lalu dipakai oleh setiap method yang membutuhkannya. Jangan mengetik ulang angka `-1.0` di dalam method.
- Rumus imputasi mengasumsikan hari sebelum dan sesudah hari yang kosong tersedia sebagai data asli (bukan ikut kosong). Data pada soal ini sudah dijamin memenuhi asumsi tersebut.
