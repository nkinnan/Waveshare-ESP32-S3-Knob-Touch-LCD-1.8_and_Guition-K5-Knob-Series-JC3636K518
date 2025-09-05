# ESPHome config for: Waveshare ESP32 S3 Knob Touch LCD 1.8 / Guition K5 Knob Series JC3636K518
Fully functional with all peripherals (goal) ESPHome config that works for both the Waveshare ESP32 S3 Knob Touch LCD 1.8 and its clone the Guition K5 Knob Series JC3636K518.  

Note that there are several similar products out there, this is the one with the rotary knob surrounding the screen (either blue for Waveshare or red for Guition), an optional internal battery, and a fully enclosed back (no exposed PCB).  If you have one of the other versions, they may use similar or the same hardware in some places, so some of this may still be useful to you, but I wanted the rotary knob so that's what I've focused on.

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

This appears to be the original source code for the software shipped on the Guition JC3636K518: 

http://pan.jczn1688.com/1/HMI%20display (file: JC3636K518CN_knob_EN.zip)

direct link: http://pan.jczn1688.com/directlink/1/HMI%20display/JC3636K518CN_knob_EN.zip or http://pan.jczn1688.com/s/54f682

I'm unable to find the original source code for the Waveshare version, only the waveshare wiki which DOES have snippets showing how to interact with (most of) the hardware (but no full software stack).  The waveshare wiki also has a full schematic diagram, links to datasheets on the peripheral chips, what appears to be their shipping firmware (bin only) and other resources.  

The JC3636K518 source code seems to be identical to the shipping Waveshare firmware (or at least an older version of it, I've seen youtube videos showing an identical UI).  I have no idea how that happened, I doubt they re-implemented it.  Perhaps there is a collaboration between the two companies?  If not, some corporate espionage or something is happening.

Ongoing Discord discussion: https://discord.com/channels/429907082951524364/1411734762002845789

misc: 

This device incorporates two different ESP chips:

Guition K5:
```
Connected to ESP32-S3 on COM4:
Chip type:          ESP32-S3 (QFN56) (revision v0.2)
Features:           Wi-Fi, BT 5 (LE), Dual Core + LP Core, 240MHz, Embedded PSRAM 8MB (AP_3v3)
Crystal frequency:  40MHz
USB mode:           USB-Serial/JTAG

Connected to ESP32 on COM5:
Chip type:          ESP32-U4WDH (revision v3.1)
Features:           Wi-Fi, BT, Dual Core + LP Core, 240MHz, Embedded Flash, Vref calibration in eFuse, Coding Scheme None
Crystal frequency:  40MHz
```

You can select which ESP to flash over the USB-C connector by using a USB-A to USB-C cable and flipping its orientation.  If using a C-C cable, which ESP you're talking to will depend on which signal line pair the host-side mux selects.  

For the Guition clone, the secondary ESP32 can have its flash read out without issue using esptool, but for the primary S3 I've had to use a retry script (converted from bash to powershell based on a script by pzich):

```
#!/usr/bin/env pwsh

$ErrorActionPreference = "Stop" # if something goes wrong, bail
$PSNativeCommandUseErrorActionPreference = $false # unless it's esptool, it can return non-zero exit and we handle that manually

# Configure this for your chip
$flash_size = 1024*1024*16
$read_size = 4096

$read_size_hex = '{0:X8}' -f $read_size # 8-digit uppercase hex with zero padding
$read_count = $flash_size / $read_size
$current_read = 0
$total_retries = 0

md -Force "binparts" >$null

# Run initial flash-id command, technically we could read the output and set $flash_size that way but it's not worth the bother
esptool -b 115200 --after no-reset-stub flash-id

while ($true) {
    if ($current_read -ge $read_count) {
        break
    }

    $current_filename = 'binparts\\{0:D8}.bin' -f $current_read # zero-padded 8-digit decimal filename

    if (Test-Path $current_filename) {
        Write-Host "Skipping $current_read..." -BackgroundColor Yellow
        $current_read++
        continue
    }

    $current_read_offset_hex = '{0:X8}' -f ($current_read * $read_size) # 8-digit uppercase hex with zero padding
    Write-Host "Reading chunk $current_read at $current_read_offset_hex..." -BackgroundColor Magenta

    $current_retries = 0
    while ($true) {
        esptool -b 115200 --before no-reset --after no-reset-stub read-flash "0x${current_read_offset_hex}" "0x${read_size_hex}" $current_filename
        
        if($LastExitCode -ne 0) {
            $total_retries++
            $current_retries++
            Write-Host "Retrying read $current_read at $current_read_offset_hex, attempt $current_retries..." -BackgroundColor Red
            continue
        }

        break
    }

    $current_read++
}

Write-Host "DONE" -BackgroundColor Green
Write-Output "Retries: $total_retries"

Write-Output "Concatenating files..."

if (Test-Path "flash.bin") {
    ri -Force "flash.bin"
}

0..($read_count - 1) | ForEach-Object {

    $part_filename = 'binparts\\{0:D8}.bin' -f $_
    $part = Get-Content -Path $part_filename
    Add-Content -Path flash.bin -Value $part
}

if (Test-Path "binparts") {
    ri -Force "binparts" -Recurse
}

Write-Output "Complete, flash.bin available in current directory, temporary files cleaned."
```

I haven't tried writing them yet but hopefully the same won't be needed to *write* the flash on the primary S3 as well...

A neat free LED lighting control UI: https://www.youtube.com/watch?v=8pHF0OAG2TI

Not the same device, but close, and may be helpful: https://community.home-assistant.io/t/waveshare-esp32-s3-lcd-1-85/833702/6

Possibly a relevant discussion about PlatformIO board definition: https://community.platformio.org/t/help-adding-board-for-waveshare-esp32-s3-with-round-1-85-display/46062/3

