# Peta Taman Kota Medan

Peta interaktif taman dan ruang terbuka hijau di Kota Medan. Projek SIG Web tahap 1, dibuat dengan Leaflet.

**Live demo:** https://elainekeishamp.github.io/index.html/

## Tujuan

Menampilkan taman dan ruang hijau di Kota Medan yang tercatat di OpenStreetMap, berapa luasnya, dan taman mana yang paling dekat dari lokasi pengguna. Topiknya terkait SDG 11 target 11.7, yaitu akses ke ruang hijau publik, dan UU 26 Tahun 2007 tentang Penataan Ruang yang mewajibkan RTH minimal 30 persen luas kota.

## Fitur

1. Lima pilihan peta dasar: Esri Jalan, Esri Satelit, Esri Satelit + Label, Esri Topografi, dan OpenTopoMap
2. Layer tematik yang bisa dinyalakan atau dimatikan: taman, kebun atau taman hias, taman bermain
3. Popup tiap taman berisi nama, jenis, luas, dan alamat, pengelola, serta jam buka kalau ada di OSM, plus link ke halaman OSM-nya
4. Pencarian taman berdasarkan nama
5. Lokasi saya, menampilkan 3 taman bernama terdekat
6. Ukur jarak dengan hasil tampil di atas peta
7. Legenda, skala peta, dan atribusi
8. Ringkasan jumlah objek, luas total, dan jumlah yang bernama
9. Unduh data dalam format GeoJSON

## Cara pakai

1. Ganti peta dasar dan layer lewat panel di sebelah kiri. Di HP panel ini ada di bawah peta.
2. Klik taman di peta untuk melihat popup, atau ketik namanya di kolom Cari taman.
3. Klik **Lokasi saya** dan izinkan akses lokasi di browser. Jarak yang ditampilkan adalah jarak garis lurus ke titik tengah taman.
4. Klik **Ukur jarak**, lalu klik titik demi titik di peta, boleh di atas taman. Jarak total tampil di kotak atas peta, dan tiap titik punya label jarak kumulatif. **Batal titik** menghapus titik terakhir, **Hapus** mengosongkan ukuran, **Selesai** mematikan mode ukur supaya popup taman bisa diklik lagi.

## Data

Semua data berasal dari OpenStreetMap (lisensi ODbL), diambil lewat Overpass Turbo dengan query berikut:

```
\[out:json]\[timeout:60];
area\["boundary"="administrative"]\["name"\~"^(Kota )?Medan$"]\["admin\_level"\~"^(5|6)$"]->.a;
(nwr(area.a)\["leisure"\~"^(park|garden|playground|nature\_reserve)$"];);
out geom;
```

Hasilnya 64 objek: 55 taman, 4 kebun atau taman hias, dan 5 taman bermain. Sebanyak 25 objek punya nama dan total luas poligonnya sekitar 90 hektar. Tidak ada titik yang dibuat manual.

Data hasil ekspor ditanam langsung di `index.html` (variabel `RTH\_GEOJSON`), jadi peta tampil tanpa bergantung pada server Overpass. Salinannya ada di `rth-medan.geojson` dan bisa dibuka di QGIS. Tombol **Ambil ulang data taman** mengambil data terbaru langsung dari Overpass dan butuh internet.

Luas dihitung di browser dari geometri poligon memakai rumus spherical shoelace dari materi kuliah. Jarak memakai fungsi `distanceTo` bawaan Leaflet.

## Teknologi

HTML, CSS, dan JavaScript biasa dalam satu berkas. Library: Leaflet 1.9.4 dan osmtogeojson 3.0.0-beta.5 (hanya dipakai untuk tombol ambil ulang data). Font Lato dari Google Fonts.

## Menjalankan di komputer

Buka `index.html` dengan klik dua kali. Koneksi internet tetap dibutuhkan untuk memuat Leaflet, font, dan peta dasar.

## Struktur berkas

```
index.html
README.md
rth-medan.geojson
```

## Keterbatasan

1. Kelengkapan data mengikuti kontribusi komunitas OSM, jadi ini bukan data resmi Pemerintah Kota Medan. Ada taman yang mungkin belum tergambar atau belum bernama.
2. Total luas sekitar 90 hektar jauh di bawah RTH Kota Medan yang sebenarnya. Angka ini tidak boleh dibaca sebagai persentase RTH kota, karena untuk itu dibutuhkan data resmi dan luas wilayah kota.
3. Taman milik swasta atau yang berpagar bisa ikut tercatat. Cek keterangan akses di popup kalau tersedia.
4. Jarak ke taman terdekat dan hasil ukur jarak berupa garis lurus, bukan jarak lewat jalan.
5. Peta dasar berasal dari layanan pihak ketiga, jadi kalau salah satunya gagal dimuat, peta otomatis pindah ke Esri Jalan.

## Kesesuaian dengan rubrik

|Kriteria|Yang dikerjakan|
|-|-|
|Peta tampil dan stabil|Peta dan basemap termuat, kalau tile gagal otomatis pindah ke Esri Jalan, error data ditampilkan sebagai pesan|
|Data spasial terikat|64 objek poligon dan titik dari OSM dengan popup berisi atribut|
|Interaktivitas|Pilihan peta dasar dan layer (dibuat sebagai panel samping, bukan kontrol bawaan Leaflet), pencarian, lokasi saya, ukur jarak|
|Desain dan aksesibilitas|Responsif sampai lebar 375 px, legenda, atribusi, kontras teks di atas 4,5:1, tombol bisa diakses keyboard|
|Dokumentasi dan publikasi|README ini dan GitHub Pages|

## Atribusi

Data taman: © OpenStreetMap contributors, lisensi ODbL. Peta dasar: Esri dan OpenTopoMap (CC-BY-SA).

