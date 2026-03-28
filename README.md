hook up the ADC like this:

BCK to RPi GPIO 18

DATA to RPi GPIO 20

LRCK to RPi GPIO 19

connect MCLK to the 24.576 MHz oscillator and select Master Mode and 24 bit operation.


compile the wm88782.dts with:

dtc -@ -I dts -O dtb -o wm8782.dtbo wm8782.dts

copy it to the overlay folder for Trixie:

sudo cp wm8782.dtbo /boot/firmware/overlays/

add "snd-soc-spdif-rx" as the last line to /etc/modules:

sudo nano /etc/modules

load the module (needed only once):

sudo modprobe snd-soc-spdif-rx

edit /boot/firmware/config.txt and remove all other sound overlays and add:

dtparam=i2s=on

dtoverlay=wm8782

reboot:

sudo reboot now

does it appear?:

arecord -l

if yes then run to test:

arecord -D hw:WM8782ADC -f S32_LE -r 96000 -c 2 -d 30 test.wav
