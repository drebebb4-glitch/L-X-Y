# PRD — LUMAIR

**Product:** LUMAIR — Clarity in Every Breath
**Tim:** Hartono Family (241110007 William Wiryawan, 241110144 Dicky Taruna, 241110506 Owen Wijaya)
**SDG:** SDG 11.6 — Kualitas udara perkotaan
**Platform wajib:** Mobile Flutter, fully work (install → jalan, bukan mockup)
**Sumber data:** API publik, GPS bebas, tanpa fitur lapor
**Design:** No Figma, AI-designed langsung di Flutter via `app_theme.dart`
**Status:** Prototype Minggu 1-2

---

## 1. Problem & Goal

Udara lagi buruk, tapi warga tidak tahu angka pastinya di lokasi dia berdiri dan harus ngapain (masker? indoor? anak aman?).

Goal: user buka LUMAIR <5 detik langsung tahu: **AQI berapa, kategori apa, polutan dominan apa, dan rekomendasi kesehatan.**

Non-goal (sengaja TIDAK dikerjakan biar fully work): login/auth, backend sendiri, lapor polusi, IoT sensor, sosial/leaderboard.

## 2. User & Use Case

Single actor: **Pengguna Umum**

1. Buka app → lihat AQI lokasi GPS saat ini
2. Cari kota lain + simpan favorit (max 5)
3. Lihat detail polutan + forecast + edukasi
4. Dapat peringatan kalau AQI >150 (V2, bukan prototype)

Use Case Diagram (textual):
- UC-1 Lihat AQI Lokasi Saat Ini (primary)
- UC-2 Cari & Simpan Lokasi Favorit (primary)
- UC-3 Lihat Prakiraan & Rekomendasi Kesehatan (primary)
- UC-4 Terima Notifikasi Bahaya (secondary, V2)

## 3. Scope MVP Fully Work

### P0 — Wajib jalan saat demo (prototype M1-2)
- [ ] Splash + onboarding izin lokasi (allow/deny dengan fallback Jakarta -6.2, 106.8166)
- [ ] Home: gauge AQI besar + kategori + warna dinamis + polutan dominan + suhu/kelembapan + jam update + pull-to-refresh + banner advice 1 baris + loading/error/empty state
- [ ] Detail: 6 kartu PM2.5, PM10, O3, NO2, SO2, CO (nilai µg/m3 + bar + deskripsi sumber & risiko)
- [ ] Search kota + Favorit lokal (Hive) + tap favorit load AQI-nya
- [ ] Map fullscreen `flutter_map` + OSM (gratis, jangan Google Maps) + pin user + pin favorit warna AQI + bottom sheet
- [ ] Forecast: grafik 24 jam `fl_chart` + list 4 hari (dari `hourly`)
- [ ] Edukasi: 6 artikel hardcode (Apa itu PM2.5, AQI, Masker, Olahraga, Anak & Asma, Sumber polusi kota)
- [ ] Cache 30 menit + dummy JSON fallback (matiin internet tetap jalan)
- [ ] Build APK debug sukses install

### P1 — V2 (M3-4 kalau sempat)
- [ ] Settings (ganti standar US/EU, unit, about sumber data)
- [ ] Local notification AQI >150 via workmanager
- [ ] Firebase Auth + sync favorit (hanya kalau dosen wajib ada login/DB)

## 4. Functional Requirements

### FR-1 Lokasi
- Package `geolocator`, `geocoding`
- `requestPermission()`, `getCurrentPosition(high)`, timeout 10 detik
- Deny/timeout → fallback Jakarta + snackbar "GPS mati, pakai Jakarta"
- Reverse geocode nama kota via `placemarkFromCoordinates`

### FR-2 API Utama: Open-Meteo (tanpa key, gratis)
```
GET https://air-quality-api.open-meteo.com/v1/air-quality?latitude={lat}&longitude={lon}&current=us_aqi,pm2_5,pm10,ozone,nitrogen_dioxide,sulphur_dioxide,carbon_monoxide,temperature_2m,relative_humidity_2m&hourly=us_aqi,pm2_5&timezone=auto
```
Fallback opsional: OpenWeather `data/2.5/air_pollution?lat&lon&appid={KEY}` (butuh key).

Cache: Hive box `lumair_cache`, TTL 30 menit, key `lat_lon_rounded_2desimal`.

Contoh fallback dummy: `assets/dummy/fallback_jakarta.json`

### FR-3 Standar AQI
Pakai US AQI 0-500. Mapping paten di `aqi_helper.dart`:
- 0-50 #22C55E Baik
- 51-100 #EAB308 Sedang
- 101-150 #F97316 Sensitif (Orange)
- 151-200 #EF4444 Tidak Sehat
- 201-300 #A855F7 Sangat Tidak Sehat
- 300+ #7F1D1D Berbahaya

### FR-4 Advice Otomatis
Rule-based dari AQI + toggle sensitif (anak/asma):
- Baik: aman outdoor
- Sedang: sensitif kurangi lama outdoor
- 101-150: pakai masker bila lama di luar, anak/asma batasi
- >150: wajib masker, hindari outdoor lama, anak/asma indoor, olahraga indoor
- Teks Bahasa Indonesia singkat, tampil di `AdviceBanner`

### FR-5 Search Kota
```
GET https://geocoding-api.open-meteo.com/v1/search?name={q}&count=5&language=id
```
Simpan max 5 favorit ke Hive.

### FR-6 Offline
Semua screen baca cache dulu, tampilkan badge "Data X jam lalu — offline" bila pakai cache/dummy.

## 5. Screens (7, AI-designed, Material 3)

1. `splash + onboarding`
2. `home` (gauge + mini cuaca + advice banner)
3. `detail` (6 pollutant_card)
4. `map` (flutter_map + bottom sheet)
5. `forecast` (chart + list)
6. `favorites + search`
7. `education list + detail` (+ `settings/about` ringan)

Design tokens di `lib/core/app_theme.dart`:
- Font: Plus Jakarta Sans
- Radius 20, Material 3, light clean putih + emerald aksen + gold tipis (premium LUMAIR)
- Komponen wajib: `AqiGauge, PollutantCard, AdviceBanner`
- Dark mode opsional V2

## 6. Tech & Arsitektur

Flutter stable, Dart 3. State: Riverpod (satu saja, jangan campur Bloc).

Dependencies:
`geolocator, geocoding, http/dio, flutter_map, latlong2, fl_chart, hive, hive_flutter, shared_preferences, intl, flutter_local_notifications (V2), workmanager (V2)`

Struktur:
```
lib/main.dart
lib/core/app_theme.dart, aqi_helper.dart, constants.dart
lib/data/api/openmeteo_api.dart, geocoding_api.dart
lib/data/services/location_service.dart, cache_store.dart
lib/domain/models/air_quality.dart, forecast.dart, favorite.dart
lib/presentation/screens/... + widgets/aqi_gauge.dart, pollutant_card.dart, advice_banner.dart
assets/dummy/fallback_jakarta.json + education.json
```

## 7. Data Model

```dart
AirQualityReading { lat, lon, placeName, time, usAqi, euAqi, pm25, pm10, o3, no2, so2, co, temp, humidity, dominant }
ForecastPoint { time, aqi, pm25 }
FavoritePlace { id, name, lat, lon, lastAqi, updatedAt }
```

## 8. NFR & Acceptance

- Cold start → AQI tampil <8 detik di 4G (cache <1 detik)
- GPS deny tetap jalan, API fail tetap jalan (dummy)
- Tidak crash di Android 10-15, izin lokasi dijelaskan
- Demo 90 detik: buka → refresh → detail PM2.5 → peta → tambah favorit Bandung → matiin data tetap jalan

## 9. Milestone (solo build-first)

- M1 (H1-7): setup + theme + location + api + home + detail jalan
- M2 (H8-14): map + favorit + forecast + edukasi + APK prototype
- Laporan bagi tugas belakangan, commit per modul biar gampang dipisah

## 10. Risiko & Mitigasi

- Emulator GPS kosong → tombol "Pakai Jakarta" + input lat/lon manual di dev menu
- Beda angka vs IQAir/BMKG → disclaimer: model CAMS estimasi, untuk edukasi bukan regulasi
- Billing map → wajib `flutter_map` OSM, jangan `google_maps_flutter`
- Flutter SDK belum ada di mesin ini → install Flutter stable + Android SDK sebelum `flutter create lumair`, atau kerjakan scaffold manual dulu

---
*Disetujui: Hartono Family. Nama produk: LUMAIR.*
