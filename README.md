# ESPHome config for: Waveshare ESP32 S3 Knob Touch LCD 1.8 / Guition K5 Knob Series JC3636K518
Fully functional with all peripherals (goal) ESPHome config that works for both the Waveshare ESP32 S3 Knob Touch LCD 1.8 and its clone the Guition K5 Knob Series JC3636K518.  

Note that there are several similar products out there, this is the one with the rotary knob surrounding the screen, an optional internal battery, and a fully enclosed back (no exposed PCB).  If you have one of the other versions, they may use similar or the same hardware in some places, so some of this may still be useful to you, but I wanted the rotary knob so that's what I've focused on.

I wouldn't be surprised if Waveshare and Guition are buying the screens and the housing from the same supplier made on the same production line.  There are some very small differences in the PCB between the two of them, enough to know it was re-laid-out or at least that the pcb files were modified between the two.  Some resistors are rotated or missing, some components have a different footprint/package, etc.  They are largely identical though, right down to the spot for the battery and the daughter PCB underneath the main PCB, even connected the same way.

Waveshare product page: https://www.waveshare.com/esp32-s3-knob-touch-lcd-1.8.htm

Waveshare wiki (WITH EXAMPLE CODE for both Arduino and ESP-IDF frameworks to interact with most of the peripherals): https://www.waveshare.com/wiki/ESP32-S3-Knob-Touch-LCD-1.8

Waveshare official store: https://www.aliexpress.com/i/3256809117538849.html

Amazon purchase link (no referral code included, this is just a place you can get it): https://www.amazon.com/dp/B0FDL4FJ13
* Note: I think this version has no battery included, but it should be possible to add one if you don't mind removing some screws.

Guition official store page: https://www.aliexpress.us/item/3256808637654546.html

Alternative product listing: https://www.surenoo.com/collections/258656/products/23666971

Amazon purchase link (no referral code included, this is just a place you can get it): https://www.amazon.com/dp/B0F8QWF7N3

Some work has already been done by KrX3D: https://github.com/KrX3D/WaveShare-Knob-Esp32S3

ESPHome discussion which got some things at least partially working: https://github.com/orgs/esphome/discussions/3253

Home Assistant discussion: https://community.home-assistant.io/t/display-knob/905249

Not the same device, but close, and may be helpful: https://community.home-assistant.io/t/waveshare-esp32-s3-lcd-1-85/833702/6

Possibly a relevant discussion about PlatformIO board definition: https://community.platformio.org/t/help-adding-board-for-waveshare-esp32-s3-with-round-1-85-display/46062/3

This appears to be the original source code for the software shipped on the Guition JC3636K518: 

http://pan.jczn1688.com/1/HMI%20display (file: JC3636K518CN_knob_EN.zip)

direct link: http://pan.jczn1688.com/directlink/1/HMI%20display/JC3636K518CN_knob_EN.zip or http://pan.jczn1688.com/s/54f682

I'm unable to find the original source code for the Waveshare version, only the waveshare wiki which DOES have snippets showing how to interact with (most of) the hardware (but no full software stack).  The waveshare wiki also has a full schematic diagram, links to datasheets on the peripheral chips, what appears to be their shipping firmware (bin only) and other resources.  

For some things like interacting with the secondary ESP which I'm guessing is handling bluetooth in parallel with the main ESP, I think it's going to require digging into the JC3636K518 source code.  Which as far as I can tell is actually identical to the shipping Waveshare firmware.  I have no idea how that happened, I doubt they re-implemented it.  Perhaps there is a collaboration between the two companies?  If not, some serious corporate espionage or something is happening.

Ongoing Discord discussion: https://discord.com/channels/429907082951524364/1411734762002845789
