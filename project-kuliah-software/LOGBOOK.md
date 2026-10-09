# Logbook Proyek — LUMAIR (Clarity in Every Breath)

| Informasi | Detail |
|---|---|
| Tim | Hartono Family |
| Anggota | 241110007 — William Wiryawan; 241110144 — Dicky Taruna; 241110506 — Owen Wijaya |
| SDG | SDG 11 — Sustainable Cities and Communities |
| Dokumen PRD | [PRD-LUMAIR.md](./PRD-LUMAIR.md) |
| Kode sumber | [`lumair/`](./lumair/) |

## Demo Aplikasi

Hasil build yang sudah berjalan, dapat dibuka langsung di peramban (HP/laptop):

**https://drebebb4-glitch.github.io/L-X-Y/lumair/**

## Pembagian Tugas

| Nama Tugas | NIM | Nama Mahasiswa | Sub Bagian Tugas | Status | Tanggal Deadline | Files | Komentar |
|---|---|---|---|---|---|---|---|
| Tampilan Beranda, Peta, API | 241110144 | Dicky Taruna | Implementasi halaman Beranda, Peta AQI, dan integrasi API udara (Open-Meteo) | Selesai | 9 Oktober 2026 | `main.dart`, `app_theme.dart`, `aqi_helper.dart`, `openmeteo_api.dart`, `geocoding_api.dart`, `location_service.dart`, `home_screen.dart`, `map_screen.dart` — [Lihat di GitHub](https://github.com/drebebb4-glitch/L-X-Y/tree/main/project-kuliah-software/lumair/lib) | 8 berkas di folder `lumair/lib` |
| Ramalan Udara, Detail Udara | 241110007 | William Wiryawan | Implementasi halaman Prakiraan 24 jam dan Detail polutan | Selesai | 9 Oktober 2026 | `cities.dart`, `constants.dart`, `air_quality.dart`, `providers.dart`, `forecast_screen.dart`, `detail_screen.dart` — [Lihat di GitHub](https://github.com/drebebb4-glitch/L-X-Y/tree/main/project-kuliah-software/lumair/lib) | 6 berkas di folder `lumair/lib` |
| Favorit, Edukasi & Mode Offline | 241110506 | Owen Wijaya | Implementasi Favorit, Edukasi, mode offline (cache), dan pengujian | Selesai | 9 Oktober 2026 | `cache_store.dart`, `favorites_screen.dart`, `education_screen.dart`, `fallback_jakarta.json`, `widget_test.dart` — [Lihat di GitHub](https://github.com/drebebb4-glitch/L-X-Y/tree/main/project-kuliah-software) | 5 berkas, buka folder lalu cari nama berkas |

## Verifikasi

- `flutter analyze` — tidak ada masalah.
- `flutter test` — 3/3 pengujian lolos.
- `flutter build web` — berhasil, dideploy ke GitHub Pages (tautan demo di atas).
