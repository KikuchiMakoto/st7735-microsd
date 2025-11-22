# st7735-microsd
sainsmart st7735-microsd module for rpi dts

```
dtc -@ -I dts -O dtb -o sainsmart-st7735-microsd.dtbo sainsmart-st7735-microsd-overlay.dts
sudo cp sainsmart-st7735-microsd.dtbo /boot/firmware/overlays/
```