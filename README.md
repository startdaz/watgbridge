# WhatsApp-Telegram-Bridge

Meskipun namanya, ini bukanlah "jembatan" yang sebenarnya. Ini meneruskan pesan dari WhatsApp ke Telegram dan Anda dapat membalasnya dari Telegram.

<a href="https://t.me/PropheCProjects">
  <img src="https://img.shields.io/badge/Updates_Channel-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"></img>
</a>&nbsp; &nbsp;
<a href="https://t.me/WaTgBridge">
  <img src="https://img.shields.io/badge/Discussion_Group-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"></img>
</a>&nbsp; &nbsp;
<a href="https://youtu.be/xc75XLoTmA4">
  <img src="https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white"</img>
</a>

## Perhatian !!!

Proyek ini sama sekali tidak terkait dengan WhatsApp atau Telegram. Menggunakan ini juga dapat menyebabkan akun Anda diblokir oleh WhatsApp, jadi gunakan dengan risiko Anda sendiri.

## Contoh Tangkapan Layar

<p align="center">
  <img src="./assets/telegram_side_sample.png" width="350" alt="Sisi Telegram">
  <img src="./assets/whatsapp_side_sample.jpg" width="350" alt="Sisi WhatsApp">
</p>

## Fitur dan Pilihan Desain

- Semua pesan dari berbagai obrolan (di WhatsApp) dikirim ke topik/threads yang berbeda dalam grup target yang sama (di Telegram)
- Opsi konfigurasi tersedia untuk menonaktifkan berbagai jenis pembaruan dari WhatsApp
- Dapat membalas dan mengirim pesan baru dari Telegram
- Dapat menandai semua orang menggunakan @all atau @everyone. Orang lain juga dapat menggunakan ini dalam obrolan grup yang Anda tentukan dalam file konfigurasi
- Dapat bereaksi terhadap pesan dengan membalas dengan satu emoji yang diinginkan
- Mendukung stiker statis dari kedua platform
- Dapat mengirim stiker Animasi (TGS) dari Telegram
- Stiker video dari sisi Telegram didukung
- Stiker video dari sisi WhatsApp saat ini diteruskan sebagai GIF ke Telegram

## Bug dan TO-DO

- Penamaan dokumen berantakan dan tidak konsisten di Telegram, perlu menemukan cara untuk selalu mengirim nama yang jelas

PR diterima :)

## Instalasi

- Buat supergrup (aktifkan riwayat pesan untuk anggota baru) dengan topik yang diaktifkan
- Tambahkan bot Anda ke dalam grup, jadikan bot sebagai admin dengan izin untuk `Kelola topik`
- Instal `git`, `gcc`, `golang`, `ffmpeg`, `imagemagick` (opsional), pada sistem Anda
- Kloning repositori ini di mana saja dan navigasikan ke direktori hasil kloning
- Jalankan `go build`
- Salin `sample_config.yaml` ke `config.yaml` dan isi nilainya, ada komentar untuk membantu Anda.
- Jalankan biner dengan menjalankan `./watgbridge`
- Pada menjalankan pertama kali, itu akan menampilkan kode QR untuk masuk ke WhatsApp yang dapat dipindai oleh aplikasi WhatsApp di `Perangkat Tertaut`
- Disarankan untuk memulai ulang bot setelah beberapa jam karena WhatsApp sering kali suka memutuskan sambungan. Jadi file layanan Systemd contoh telah disediakan (`watgbridge.service.sample`). Edit `User` dan `ExecStart` sesuai dengan pengaturan Anda:
    - Jika Anda tidak memiliki server API bot lokal, hapus `tgbotapi.service` dari kunci `After` di bagian `Unit`.
    - File layanan ini akan memulai ulang bot setiap 24 jam




# 🚀 Panduan Deploy Bot WhatsApp `watgbridge` di VPS Ubuntu 20.04

## 1. Menyiapkan VPS Ubuntu 20.04

Setelah mendapatkan akses ke VPS, ikuti langkah-langkah berikut untuk memulai:

### 1.1. Update dan Upgrade Sistem 🛠️

```bash
sudo apt update && sudo apt upgrade -y
```

### 1.2. Instalasi Alat Dasar 📦

```bash
sudo apt install -y git curl wget build-essential ufw
```

## 2. Konfigurasi Firewall 🔒 (Optional, tapi disarankan)

Menggunakan `ufw` (Uncomplicated Firewall) untuk mengelola firewall:

### 2.1. Mengaktifkan Firewall

```bash
sudo ufw allow OpenSSH; sudo ufw enable
```

### 2.2. Izinkan Port Lain (Jika Diperlukan)

```bash
sudo ufw allow 80/tcp; sudo ufw allow 443/tcp
```

## 3. Instalasi Go 🏗️

Instal Go, yang diperlukan untuk membangun dan menjalankan aplikasi Go seperti `watgbridge`:

### 3.1. Instal Go dengan `snap`

```bash
sudo snap install go --classic
```

### 3.2. Verifikasi Instalasi Go

```bash
go version
```

Output yang diharapkan:

```bash
go version go1.23.0 linux/amd64
```

## 4. Kloning Repository `watgbridge` 📂

Berikutnya, kita akan mengkloning repository `watgbridge` ke VPS:

### 4.1. Kloning Repository

```bash
cd ~; git clone https://github.com/akshettrj/watgbridge.git; cd watgbridge
```

## 5. Konfigurasi Proyek ⚙️

Sebelum menjalankan proyek, kita perlu mengonfigurasi file `config.yaml`.

### 5.1. Edit File Konfigurasi

Gunakan editor teks favorit Anda (misalnya `nano`) untuk mengedit `config.yaml`:

```bash
nano config.yaml
```

### 5.2. Masukkan Konfigurasi

- **Telegram**: Masukkan token API bot Telegram Anda.
- **WhatsApp**: Masukkan konfigurasi yang diperlukan untuk WhatsApp.

Contoh konfigurasi sederhana:

```yaml
telegram:
  bot_token: 186779
  #api_url: http://localhost:8082        # Uncomment if you have a local bot API server running (for bypassing file size limits)
  self_hosted_api: false
  owner_id: 704338780
  sudo_users_id:
    - 704338780
  target_chat_id: -100423424              # This is the chat where messages will be forwarded (note the "100" prefix of a supergroup)
  skip_video_stickers: false              # Setting this as true will stop trying to convert telegram video stickers to webp and sending them
  skip_setting_commands: false            # This will not show you list of commands when you start typing / in telegram

  send_my_presence: false                 # Setting this to true will show your account as online to others whenever you send a message using Telegram
  send_my_read_receipts: false            # Setting this to true will mark all unread messages in a chat as read when you send a new message using Telegram

  silent_confirmation: true               # Send a silent "Successfully sent" message
  emoji_confirmation: true                # Reacts to the message with a "👍" emoji instead of replying

  skip_startup_message: false             # If set to true, then a message will NOT be sent to your Telegram DM when the bot starts
```

Simpan dan keluar (`Ctrl+O`, `Enter`, `Ctrl+X` jika menggunakan `nano`).

## 6. Mengelola Dependensi dan Build Proyek 🔧

Sekarang kita perlu memastikan bahwa semua dependensi tersedia dan proyek dibangun dengan benar.

### 6.1. Menyiapkan Dependensi

```bash
go mod tidy
```

### 6.2. Build Proyek

```bash
go build .
```

Ini akan menghasilkan file eksekusi di direktori proyek.

## 7. Menjalankan Aplikasi ▶️

Setelah build selesai, jalankan aplikasi untuk mulai menjembatani pesan antara Telegram dan WhatsApp.

### 7.1. Jalankan Aplikasi

```bash
./watgbridge
```

### 7.2. Memastikan Aplikasi Berjalan di Background 🌐

Untuk memastikan aplikasi terus berjalan meskipun Anda keluar dari sesi SSH, jalankan aplikasi menggunakan `tmux` atau `screen`.

Contoh menggunakan `tmux`:

```bash
sudo apt install tmux -y; tmux new -s watgbridge; ./watgbridge
```

Untuk keluar dari sesi tmux sementara tetap menjalankan aplikasi, tekan `Ctrl+B` diikuti oleh `D`.

Anda bisa kembali ke sesi `tmux` kapan saja dengan:

```bash
tmux attach-session -t watgbridge
```

## 8. Menyiapkan Aplikasi Sebagai Layanan Sistem (Opsional) ⚙️

Jika Anda ingin aplikasi berjalan otomatis saat VPS booting, Anda bisa mengkonfigurasi `systemd` service:

### 8.1. Buat File Service

Buat file service baru:

```bash
sudo nano /etc/systemd/system/watgbridge.service
```

Isi dengan konfigurasi berikut:

```ini
[Unit]
Description=Watgbridge Service
After=network.target

[Service]
ExecStart=/root/watgbridge/watgbridge
WorkingDirectory=/root/watgbridge
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

### 8.2. Reload dan Aktifkan Service

```bash
sudo systemctl daemon-reload; sudo systemctl enable watgbridge; sudo systemctl start watgbridge
```

### 8.3. Periksa Status Layanan

Untuk memeriksa apakah layanan berjalan dengan benar:

```bash
sudo systemctl status watgbridge
```

## 9. Monitoring dan Logging 📊

Pastikan Anda memonitor log dari aplikasi untuk mendeteksi masalah.

### 9.1. Melihat Log

Gunakan `journalctl` untuk melihat log dari service:

```bash
sudo journalctl -u watgbridge -f
```

Ini akan menampilkan log secara real-time.

## ✅ Kesimpulan

HIDUP LU KEK KONTOL, SEKIAN 👍👍👍👍👍
