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
| Tampilan Beranda, Peta, API | 241110144 | Dicky Taruna | Implementasi halaman Beranda, Peta AQI, dan integrasi API udara (Open-Meteo) | Selesai | 9 Oktober 2026 | [Dokumentasi-Prompt-241110144-Dicky-Taruna.docx](./dokumen/Dokumentasi-Prompt-241110144-Dicky-Taruna.docx) | Berisi konteks, batasan, dan 5 iterasi prompt; kode di folder `lumair/lib` |
| Ramalan Udara, Detail Udara | 241110007 | William Wiryawan | Implementasi halaman Prakiraan 24 jam dan Detail polutan | Selesai | 9 Oktober 2026 | [Dokumentasi-Prompt-241110007-William-Wiryawan.docx](./dokumen/Dokumentasi-Prompt-241110007-William-Wiryawan.docx) | Berisi konteks, batasan, dan 5 iterasi prompt; kode di folder `lumair/lib` |
| Favorit, Edukasi & Mode Offline | 241110506 | Owen Wijaya | Implementasi Favorit, Edukasi, mode offline (cache), dan pengujian | Selesai | 9 Oktober 2026 | [Dokumentasi-Prompt-241110506-Owen-Wijaya.docx](./dokumen/Dokumentasi-Prompt-241110506-Owen-Wijaya.docx) | Berisi konteks, batasan, dan 5 iterasi prompt; kode di `lumair/lib`, `lumair/assets`, `lumair/test` |

## Verifikasi

- `flutter analyze` — tidak ada masalah.
- `flutter test` — 3/3 pengujian lolos.
- `flutter build web` — berhasil, dideploy ke GitHub Pages (tautan demo di atas).
