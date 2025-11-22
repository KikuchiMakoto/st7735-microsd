# st7735-microsd
sainsmart st7735-microsd module for rpi dts

for build(convert) dts to dtbo & copy to overlays dir
```txt
dtc -@ -I dts -O dtb -o sainsmart-st7735-microsd.dtbo sainsmart-st7735-microsd-overlay.dts
sudo cp sainsmart-st7735-microsd.dtbo /boot/firmware/overlays/
```

About config.txt
```txt
dtoverlay=sainsmart-st7735-microsd,rotate=270
```

About Font
```sh
cd ~
wget https://github.com/tommythorn/spleentt-5x8-font/raw/refs/heads/main/SpleenttMedium.psf
gzip SpleenttMedium.psf
sudo mkdir -p /usr/share/consolefonts/
sudo cp SpleenttMedium.psf.gz /usr/share/consolefonts/
rm SpleenttMedium.psf.gz
```

/etc/systemd/system/set-spleentt.service
```service
[Unit]
Description=Set SpleenttMedium console font for framebuffer
After=local-fs.target
Before=getty@tty1.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/bin/sh -c 'test -f /usr/share/consolefonts/SpleenttMedium.psf.gz || exit 0; con2fbmap 1 1 || true; /usr/bin/setfont -C /dev/tty1 /usr/share/consolefonts/SpleenttMedium.psf.gz || true'

[Install]
WantedBy=multi-user.target
```

```sh
sudo systemctl daemon-reload
sudo systemctl enable --now set-spleentt.service
sudo systemctl status set-spleentt.service
```
