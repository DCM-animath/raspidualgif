# 🐍 Raspberry Pi DUAL Boot GIF 

Tampilkan **GIF animasi saat booting Raspberry Pi** menggunakan `mpv`. Tutorial ini akan memutar file GIF secara fullscreen setelah sistem masuk ke tampilan grafis (GUI). Repo ini khusus untuk 2 screen berbeda gif

## Instalasi

**Update dan install package yang dibutuhkan:**

```bash
sudo apt update
sudo apt install mpv -y
sudo apt install wlr-randr wayland-utils -y
echo "$WAYLAND_DISPLAY" 
wlr-randr

```
## 2 Monitor / Asymetric GIF
1. Buat service dan config untuk monitor 1 :
   
   Buat service:
```bash
sudo nano /etc/systemd/system/splash-left.service
```
  Lalu isi confignya :
  
```bash
[Unit]
Description=LEFT GIF (Wayland) on HDMI-A-1
After=systemd-logind.service graphical.target
Requires=systemd-logind.service
Wants=graphical.target

[Service]
User=pi
# Pastikan folder runtime user ada (1000 = UID 'pi' biasanya)
Environment=XDG_RUNTIME_DIR=/run/user/1000
Environment=WAYLAND_DISPLAY=wayland-0
ExecStart=/bin/sh -lc 'sleep 2; mpv --no-terminal --no-osc --no-audio --fs --fs-screen-name=HDMI-A-1 --loop-file=inf --really-quiet /boot/firmware/left.gif'
Restart=no

[Install]
WantedBy=graphical.target
```
2. Buat service dan config untuk monitor 2 :
   
   Buat service:
```bash
sudo nano /etc/systemd/system/splash-right.service
```
  Lalu isi confignya :
```bash
[Unit]
Description=RIGHT GIF (Wayland) on HDMI-A-2
After=systemd-logind.service graphical.target
Requires=systemd-logind.service
Wants=graphical.target

[Service]
User=pi
# Pastikan folder runtime user ada (1000 = UID 'pi' biasanya)
Environment=XDG_RUNTIME_DIR=/run/user/1000
Environment=WAYLAND_DISPLAY=wayland-0
ExecStart=/bin/sh -lc 'sleep 2; mpv --no-terminal --no-osc --no-audio --fs --fs-screen-name=HDMI-A-2 --loop-file=inf --really-quiet /boot/firmware/right.gif'
Restart=no

[Install]
WantedBy=graphical.target

```
3. Aktifkan config dan system lalu reboot:
```bash
sudo systemctl daemon-reload
sudo systemctl enable splash-left.service splash-right.service
sudo reboot
```

note: tested smoothly in raspi 5, apabila pakai raspi zero 2, tolong sleepnya diubah jadi 10


# happy coding pakde

by:andre angin







