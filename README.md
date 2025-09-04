# Waveshare ESP32 S3 Knob Touch LCD 1.8 / Guition K5 Knob Series JC3636K518
Fully functional with all peripherals (goal) ESPHome config that works for both the Waveshare ESP32 S3 Knob Touch LCD 1.8 and its clone the Guition K5 Knob Series JC3636K518.  

Note that there are several similar products out there, this is the one with the rotary knob surrounding the screen, an optional internal battery, and a fully enclosed back (no exposed PCB).  If you have one of the other versions, they may use similar or the same hardware so some of this may still be useful to you, but I wanted the rotary knob so that's what I've focused on.

I wouldn't be surprised if Waveshare and Guition are buying the screens and the housing from the same supplier made on the same production line.  There are some very small differences in the PCB between the two of them, enough to know it was re-laid-out or at least that the pcb files were modified between the two.  Some resistors are rotated or missing, some components have a different footprint/package, etc.  They are largely identical though, right down to the spot for the battery and the daughter PCB underneath the main PCB, even connected the same way.

Waveshare product page: https://www.waveshare.com/esp32-s3-knob-touch-lcd-1.8.htm

Waveshare wiki (WITH EXAMPLE CODE for both Arduino and ESP-IDF frameworks to interact with all the peripherals): https://www.waveshare.com/wiki/ESP32-S3-Knob-Touch-LCD-1.8

Amazon purchase link (no referral code included, this is just a place you can get it): https://www.amazon.com/dp/B0FDL4FJ13
* Note: I think this version has no battery included, but it should be possible to add one if you don't mind removing some screws.

Guition product page (there are multiple aliexpress pages but most are dead): https://www.surenoo.com/collections/258656/products/23666971

Amazon purchase link (no referral code included, this is just a place you can get it): https://www.amazon.com/dp/B0F8QWF7N3

Some work has already been done by KrX3D: https://github.com/KrX3D/WaveShare-Knob-Esp32S3

ESPHome discussion which got some things at least partially working: https://github.com/orgs/esphome/discussions/3253

Home Assistant discussion: https://community.home-assistant.io/t/display-knob/905249

This appears to be the original source code for the software shipped on the Guition JC3636K518: 

http://pan.jczn1688.com/1/HMI%20display (file: JC3636K518CN_knob_EN.zip)

direct link: http://pan.jczn1688.com/directlink/1/HMI%20display/JC3636K518CN_knob_EN.zip or http://pan.jczn1688.com/s/54f682

