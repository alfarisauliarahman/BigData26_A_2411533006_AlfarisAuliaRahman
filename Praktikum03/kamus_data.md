# Kamus Data Tabel Analitik Pricing

Unit pengamatan: satu borough asal per jam waktu lokal America/New_York.
Tabel yang dipakai: tabel_analitik.parquet; tabel_analitik_modul.parquet mempertahankan agregasi asli modul.

| Kolom | Tipe aktual | Satuan | Sumber dan penghitungan | Validitas dan nilai hilang |
|---|---|---|---|---|
| borough_naik | object | kategori | Taxi Zone Lookup, join PULocationID ke LocationID | Lima borough resmi; lokasi tidak diketahui dikecualikan dari pricing |
| jam_mulai | datetime64[us] | jam lokal New York | TLC pickup datetime dibulatkan ke bawah per jam | Januari 2023; bukan timestamp UTC; nilai kosong dikecualikan |
| jumlah_perjalanan | int64 | perjalanan | Hitung baris transaksi bersih per borough-jam | Minimal 30; kunci transaksi dideduplikasi |
| rata_tarif_per_mil | float64 | USD/mil | Median per kelompok dari total_amount/trip_distance, dibulatkan 3 desimal pada K-9 | Positif; MEDIAN meskipun nama kolom rata; mencakup tip dan biaya dalam total_amount |
| rata_kecepatan | float64 | mil/jam | Median per kelompok dari trip_distance/(durasi_menit/60), dibulatkan 2 desimal | Positif; MEDIAN meskipun nama kolom rata |
| suhu_c | float64 | derajat Celsius | Open-Meteo temperature_2m per jam; nilai first karena cuaca sama per jam | Cuaca tidak tersedia dikecualikan dari pricing, tanpa imputasi |
| hujan | int8 | biner 0/1 | 1 bila precipitation >0,1 mm per jam; max per kelompok | Hanya jam dengan precipitation tersedia; nilai hilang bukan 0 |
