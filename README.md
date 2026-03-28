hook up the ADC like this:

BCK to RPi GPIO 18
DATA to RPi GPIO 20
LRCK to RPi GPIO 19

connect MCLK to the 24.576 MHz oscillator and select Master Mode and 24 bit operation.

compile the wm88782.dts with:
<b>dtc -@ -I dts -O dtb -o wm8782.dtbo wm8782.dts</b>

copy it to the overlay folder for Trixie:
<b>sudo cp wm8782.dtbo /boot/firmware/overlays/</b>

add "snd-soc-spdif-rx" as the last line to /etc/modules:
<b>sudo nano /etc/modules</b>

load the module (needed only once):
<b>sudo modprobe snd-soc-spdif-rx</b>

edit /boot/firmware/config.txt and remove all other sound overlays and add:
<b>dtparam=i2s=on
dtoverlay=wm8782</b>

reboot:
<b>sudo reboot now</b>

does it appear?:
<b>arecord -l</b>

if yes then run to test:
<b>arecord -D hw:WM8782ADC -f S32_LE -r 96000 -c 2 -d 30 test.wav</b>
