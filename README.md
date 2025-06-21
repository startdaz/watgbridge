# WhatsApp-Telegram-Bridge

Despite the name, its not exactly a "bridge". It forwards messages from WhatsApp to Telegram and you can reply to them
from Telegram.

<a href="https://t.me/PropheCProjects">
  <img src="https://img.shields.io/badge/Updates_Channel-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"></img>
</a>&nbsp; &nbsp;
<a href="https://t.me/WaTgBridge">
  <img src="https://img.shields.io/badge/Discussion_Group-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"></img>
</a>&nbsp; &nbsp;
<a href="https://youtu.be/xc75XLoTmA4">
  <img src="https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white"</img>
</a>

# DISCLAIMER !!!

This project is in no way affiliated with WhatsApp or Telegram. Using this can also lead to your account getting banned by WhatsApp so use at your own risk.

## Sample Screenshots

<p align="center">
  <img src="./assets/telegram_side_sample.png" width="350" alt="Telegram Side">
  <img src="./assets/whatsapp_side_sample.jpg" width="350" alt="WhatsApp Side">
</p>

## Features and Design Choices

- All messages from various chats (on WhatsApp) are sent to different topics/threads within the same target group (on Telegram)
- Configuration options available to disable different types of updates from WhatsApp
- Can reply and send new messages from Telegram
- Can tag all people using @all or @everyone. Others can also use this in group chats which you specify in configuration file
- Can react to messages by replying with single instance of the desired emoji
- Supports static stickers from both ends
- Can send Animated (TGS) stickers from Telegram
- Video stickers from Telegram side are supported
- Video stickers from WhatsApp side are currently forwarded as GIFs to Telegram

## Bugs and TODO

- Document naming is messed up and not consistent on Telegram, have to find a way to always send same names

PRs are welcome :)


## Installation

- Make a supergroup (enable message history for new members) with topics enabled
- Add your bot in the group, make it an admin with permissions to `Manage topics`
- Install `git`, `gcc` and `golang`, `ffmpeg` , `imagemagick` (optional), on your system
- Clone this repository anywhere and navigate to the cloned directory
- Run `go build`
- Copy `sample_config.yaml` to `config.yaml` and fill the values, there are comments to help you.
- Execute the binary by running `./watgbridge`
- On first run, it will show QR code for logging into WhatsApp that can by scanned by the WhatsApp app in `Linked devices`
- It is recommended to restart the bot after every few hours becuase WhatsApp likes to disconnect a lot. So a sample Systemd service file has been provided (`watgbridge.service.sample`). Edit the `User` and `ExecStart` according to your setup:
    - If you do not have local bot API server, remove `tgbotapi.service` from the `After` key in `Unit` section.
    - This service file will restart the bot every 24 hours


# Installation STEP BY STEP !!!

## install untuk step pertama:
- `sudo apt update && sudo apt install -y git gcc make golang-go ffmpeg imagemagick`
> Cek: go version → go1.24.4 linux/amd64

## (OPSIONAL tapi dianjurkan) jalankan sebagai user non-root:
- `sudo adduser --disabled-password --gecos "" watgbridge`
- `sudo -iu watgbridge`

## ambil & kompilasi:
- `git clone https://github.com/akshettrj/watgbridge.git;cd watgbridge`
- `go mod tidy`          # opsional, merapikan module cache
- `go build -o watgbridge`
> Peringatan seperti -Wunused-value dan .note.GNU-stack boleh diabaikan; kode tetap valid.

## salin & tempel konfigurasi:
- `cp sample_config.yaml config.yaml`
- `nano config.yaml`     # isi token bot Telegram, ID group, dll.

## run pertama kali atau jika sudah punya database untuk login WhatsApp (QR-code):
- `./watgbridge`
> Terminal akan menampilkan QR-code (ASCII).
> Buka WhatsApp → Linked devices → tambahkan, lalu scan.
> Setelah tersambung, Anda akan melihat log:
`successfully logged into WhatsApp
successfully logged into telegram`
> Stop dengan Ctrl-C. Bot sudah menyimpan sesi WA di folder state/.

## (OPSIONAL) kalau sudah OK, jalankan sebagai service atau bisa gunakan mode screen TMUX/SCREEN:
- `sudo cp watgbridge.service.sample /etc/systemd/system/watgbridge.service`
- `sudo nano /etc/systemd/system/watgbridge.service`  # sesuaikan User, WorkingDirectory, dll.
- `sudo systemctl daemon-reload`
- `sudo systemctl enable --now watgbridge`
