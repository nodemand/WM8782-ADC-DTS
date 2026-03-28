The DTS file is for this ADC:<br>
https://www.audiophonics.fr/en/devices-hifi-audio-adc/adc-analog-to-digital-converter-wm8782-i2s-24bit-192khz-p-13351.html

hook up the ADC like this:<br>
<b>
<ul>
<li>BCK to RPi GPIO 18</li>
<li>DATA to RPi GPIO 20</li>
<li>LRCK to RPi GPIO 19</li>
</ul>
</b>
<ol>
<li>Use the jumper to connect MCLK to the 24.576 MHz oscillator.</li>
<li>Remove the kHz jumper entirely.</li>
<li>Select Master Mode and 24 bit operation.</li>
</ol>

compile wm8782.dts with:<br>
<b> dtc -@ -I dts -O dtb -o wm8782.dtbo wm8782.dts</b>

copy it to the overlay folder for Trixie:<br>
<b> sudo cp wm8782.dtbo /boot/firmware/overlays/</b>

edit /boot/firmware/config.txt:<br>
<b>sudo nano /boot/firmware/config.txt</b>

remove or disable all other sound overlays and comment out dtparam=audio=on:<br>
<b>#dtparam=audio=on</b>

now add:<br>
<b>dtparam=i2s=on<br>
dtoverlay=wm8782</b>

save the file and reboot:<br>
<b> sudo reboot now</b>

does it appear?:<br>
<b> arecord -l</b>

it should say something like:<br>
<b>card 0: WM8782ADC [WM8782-ADC], device 0: bcm2835-i2s-dir-hifi dir-hifi-0 [bcm2835-i2s-dir-hifi dir-hifi-0]<br>
  Subdevices: 1/1<br>
  Subdevice #0: subdevice #0<br></b>

if yes then run to test:<br>
<b> arecord -D hw:WM8782ADC -f S32_LE -r 48000 -c 2 -d 30 test.wav</b>
