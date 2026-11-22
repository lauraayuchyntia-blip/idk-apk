# Kangetsu Lyrics — versi LeviLauncher (Android, `.levipack`)

Dua bagian, keduanya jalan di HP yang sama:

    app Android "Companion"  --127.0.0.1:19133-->  mod native (.levipack)  -->  HUD Minecraft
    (baca lagu + ambil lirik)   GET /now, /lyrics    (menu setting + gambar teks)

## 1. Companion APK  (`android/`)
Build tanpa Android Studio: push folder ini ke GitHub, tab **Actions** → "Build companion APK" → unduh APK.
Pasang APK, buka app-nya, lalu:
1. **Open notification access** → aktifkan "Kangetsu Lyrics Companion" (wajib, ini yang mengizinkan membaca lagu).
2. **Battery optimisation** → izinkan tanpa batas agar tidak dimatikan sistem.
3. Putar lagu (Spotify/YouTube Music/dll). Tombol **Refresh** harus menampilkan judulnya.

## 2. Mod native  (`mod/`)
Ikuti `mod/MyMod.patch.md` (pakai template resmi + 3 edit kecil), build `.levipack` lewat workflow template,
impor lewat LeviLauncher → Mods → Add Mod. Settings ada di Mod Menu (default tombol M), modul "Kangetsu Lyrics".

## Pemetaan setting Java → Levi
enable (toggle modul) · show next line · song title intro · size · timing offset · posisi X/Y · warna · port.
Belum ada: preset, karaoke, efek hujan/awan, sumber Spotify OAuth (tidak perlu: companion membaca media session).

## Status verifikasi (jujur)
| Bagian | Status |
|---|---|
| Protokol, pencarian baris, jam lagu (`Protocol.hpp`) | **Dites** (g++ di host) |
| Parser warna | **Dites** |
| `Engine.cpp` | Lolos compile hanya terhadap **stub buatan saya**, bukan SDK asli |
| Kode Kotlin companion | **Belum di-compile maupun dites** |
| Seluruh alur di HP | **Belum pernah dijalankan** |

Hal yang kemungkinan perlu kamu koreksi saat build pertama (sengaja dikumpulkan di tempat kecil):
- `makeText()` di `Engine.cpp`: nama field teks/ukuran/warna pada `DrawCommand` **tebakan** (docs hanya menunjukkan field gambar).
- Tanda tangan `.config(...)` untuk Toggle/Color/Text (apakah min/max boleh dikosongkan), format nilai Color, dan `getResourceDir()`/`getLogger().info()` di `MyMod.patch.md`.
- Apakah `httpGet` ke `http://127.0.0.1` (cleartext) diizinkan di dalam proses launcher. Jika gagal, kita pindah ke socket langsung.
- Apakah `submitDrawCommands` aman dipanggil dari thread selain thread game.
- Versi Gradle/AGP di `android/` mungkin perlu dinaikkan.
Cek header asli `include/pl/ModMenu.hpp` yang diunduh template saat build pertama.

## Lisensi
Kode ditulis sendiri. `assets/lyrics.ttf` = Hanken Grotesk (SIL OFL 1.1, lisensi disertakan).
Mod Java asli "All Rights Reserved": minta izin Kangetsu sebelum mempublikasikan sesuatu yang menirunya.
