# Lazer.id — WhatsApp Personal Bot

Starter project pribadi berbasis Node.js + Baileys. Dashboard web menyediakan status, QR, pairing code, dan daftar media yang disimpan.

## Fitur
- `.menu`, `.ping`
- AI chat pribadi (opsional, membutuhkan OpenAI API key dan biaya API)
- Anti-link grup (bot harus admin; admin grup dikecualikan)
- Welcome & goodbye
- Game tebak angka (`.tebak`, `.jawab 7`)
- Simpan media WhatsApp yang diterima dengan membalas media menggunakan `.save`
- Dashboard web dengan HTTP Basic Authentication

## Penting sebelum deploy
1. Baileys adalah library tidak resmi yang menghubungkan WhatsApp Web. Bukan produk Meta/WhatsApp. Penggunaan otomatis dapat berisiko memutus sesi atau membatasi akun. Hindari spam.
2. Hosting gratis bukan berarti 24/7. Render Free dapat tidur setelah 15 menit tanpa trafik dan filesystem lokalnya sementara. Karena sesi WhatsApp tersimpan di `session/`, sesi bisa hilang saat restart/sleep/redeploy. Jadi Render Free cocok untuk uji coba, bukan bot permanen. Lihat https://render.com/docs/free
3. Untuk bot yang benar-benar selalu aktif, gunakan VPS/worker yang selalu hidup dan penyimpanan persisten.
4. Downloader pada starter ini menyimpan media yang dikirim ke bot; bukan pengunduh tautan YouTube/TikTok. Jangan unduh atau sebarkan konten tanpa izin.
5. Jangan pernah mengunggah `.env` atau folder `session/` ke GitHub.

## Deploy dari HP tanpa Termux (uji coba)
1. Buat akun GitHub dan repository baru bernama `lazer-id-whatsapp-bot`.
2. Upload seluruh isi folder proyek ini ke repository (jangan upload `.env`, `session/`, atau `node_modules/`).
3. Buka https://render.com dan hubungkan repository GitHub.
4. Buat **Web Service**. Build command: `npm install`; Start command: `npm start`; Runtime: Node.
5. Tambahkan environment variables:
   - `DASHBOARD_USER` = nama login yang kamu inginkan
   - `DASHBOARD_PASSWORD` = password acak yang kuat
   - `BOT_NAME` = Lazer.id
   - `OPENAI_API_KEY` = API key jika ingin AI (opsional)
   - `OPENAI_MODEL` = gpt-4o-mini (opsional)
6. Setelah deploy, buka URL Render. Browser akan meminta username dan password dashboard.
7. Tekan **Mulai / sambungkan bot**, lalu pilih QR atau pairing code.
8. Di WhatsApp: Perangkat tertaut → Tautkan perangkat. Jangan bagikan QR/pairing code kepada orang lain.

## Catatan teknis
- Node.js 20+
- `OWNER_NUMBER` masih disiapkan sebagai konfigurasi opsional; fitur otorisasi perintah owner belum diaktifkan.
- AI hanya membalas chat pribadi saat `OPENAI_API_KEY` tersedia.
- Fitur anti-link hanya dapat menghapus pesan jika bot admin grup.
- Untuk penggunaan jangka panjang, pindahkan auth state dari filesystem ke penyimpanan persisten/database dan siapkan backup aman.
