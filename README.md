# Net-Mesh-Forest

Our mission is to implement solutions dedicated to the conservation of wildlife and to ensure the survival of these species. It is also important to ensure our implementation is effective and cost efficient. 

### Disclaimer:

Use of this tool for malicious purposes is not recommended. Author(s) is/are not responsible or liable for any damage or actions to any device compromised. It is intended for educational purpose only.

## Whats Needed:

-Wyzecam V2

-2 x MicroSD Cards

## Attention: Do not install the latest Firmware on your Device. This hack will not be possible.

### How to install Custom Firmware(CFW)

The CFW consists of two parts:

1. The Custom Firmware, which doesn't contain any custom software, allows you to boot from a Micro SD card. This will only need to be done once. It simply alters the stock firmware to give you the ability to boot from a Micro SD card and needs to be flashed over the stock firmware. You can still boot the stock firmware if you remove the Micro SD card.
2. The CFW Files, which contains the custom software. You will have to install them onto your Micro SD card after you complete step 1. You can easily modify the software by editing the files on the Micro SD card.

## Installation of the microSD bootloader

1. Download the CFW-Binary for your Camera

    Name | SHA3-256
    --- | ---
    [Wyzecam V2](https://github.com/EliasKotlyar/Xiaomi-Dafang-Hacks/raw/master/hacks/cfw/wyzecam_v2/cfw-1.2.bin) | 3b2deb32d0cd3ef75afef8788854883d868c09cf78c690f4b78fc26862793af3

2. Format your microSD to FAT32. NTFS, EXFAT etc. won't work. Try to use smaller SD cards like 512 MB or create just a single primary 512 MB partition on it for maximum success rate.
3. Copy the CFW-Binary from step 1 to the formatted microSD card and rename it to "demo.bin". It is important that there must not be other files on the microSD! 
4. Remove the power cable from the camera and plug the microSD card into the camera
5. Hold down the setup button on the camera while
6. Plugging in the USB power cable
7. Keep the setup button pressed for another 10 seconds
8. Wait until the firmware has finished flashing (a few minutes).
9. Remove the microSD card and power up the camera
10. You should see the blue led shining for 5 seconds. If not, something went wrong. You should try another microSD card and look at the community tips at the bottom of the page. Start over from step 1.

## Installation of the new Firmware

1. Clone the repository from github. If you are on Windows download the repository as zip file.
2. Copy everything from "firmware_mod" folder into the **root** of the microSD

It should look like this:

```
E:/
├── autoupdate.sh
├── bin
├── config
├── controlscripts
├── driver
├── media
├── run.sh
├── scripts
├── uEnv.bootfromnand.txt
├── uEnv.bootfromsdcard.txt
├── uboot-flash
└── www

```

3. Go into config directory and rename **wpa_supplicant.conf.dist** to **wpa_supplicant.conf**
4. Modify the file config/wpa_supplicant.conf on the microSD card to match your wifi-settings. Make sure wpa_supplicant.conf does not have windows line endings.
5. Insert the microSD card and power up the camera.
6. You can now login at <https://dafang> or your cameras ip adress with the default credentials root/ismart12

Hint: The security warning about the unsafe https certificate can safely be ignored. A self-signed certificate is automatically generated on your camera during the first startup. By its nature your little camera's own certificate authority is not and never will be among the trusted ones delivered with the major browsers.

## Uninstallation

Remove the "run.sh" file from the microSD card.




## Community Tips

1. Use microSD cards smaller than 1 GB such as 512 MB and overwrite the same cards to minimize variations. Formatting only the first 512 MB has also worked for some people.
2. If the bootloader step is not working, double check the microSD card again for files or folders created by the stock firmware. (Sometimes if your timing is off with the setup press the camera will create a time stamp related folder that needs to be deleted before trying again).
3. Make a note of the MAC for the camera and if possible set up DHCP to assign a specific IP address that can be monitored visually in DHCP logs.
4. Start with fewer entries in your wpa_supplicant.conf to isolate WiFi issues.
5. SD card troubles may be fixed by formatting the card using the <https://www.sdcard.org/downloads/formatter/>official SD Memory Card Formatter</a>

```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=0
ap_scan=1

network={
 ssid="enteryourssidherebutrememebertokeepthequotes"
 psk="enteryourpasswordherebutremembertokeepthequotes"
  key_mgmt=WPA-PSK
}
```


5. Inspect you sdcard for logs/startup.log

If your Wi-Fi WPA setup uses protocol WPA2, add proto=WPA2 under ssid=
```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=0
ap_scan=1

network={
ssid="enteryourssidherebutrememebertokeepthequotes"
        # Uncomment to connect to Hidden SSIDs
        #scan_ssid=1
        key_mgmt=WPA-PSK
        pairwise=CCMP TKIP
        group=CCMP TKIP WEP104 WEP40
        psk="enteryourpasswordherebutremembertokeepthequotes"
        priority=2
        proto=WPA2
}
```

5. Inspect you sdcard for logs/startup.log











### Which Features does the CFW contain?

- Full working RTSP with H264/MJPEG. Based on <https://github.com/mpromonet/v4l2rtspserver>
- SSH-Server(dropbear) with username: root password: ismart12
- FTP-Server(bftpd) with username: root password: ismart12
- Webserver(lighttpd) with username: root password: ismart12
- Image-Snap(Get Jpeg Image)
- Horizontal/vertical motor rotation / move to center
- Turn on/off blue/yellow/IR LEDs/IR-Cut
- Local h264 recording possible:

``` sh
/system/sdcard/bin/h264Snap > /system/sdcard/video.h264
```

- Audio recording/playing possible:

```
Playing Audio:
/system/sdcard/bin/audioplay /usr/share/notify/CN/init_ok.wav volume

Recording Audio:
/system/sdcard/bin/ossrecord /system/sdcard/test.wav 
```

- Curl
- MQTT
- Telegram

- Any additional software that you may want can be compiled separately, and there is a toolchain available but you will need to do this yourself.

## What are the URLs I can use to integrate with the camera?

| Type | URLs |
| --- | --- |
| RTSP video stream | `rtsp://dafang/unicast` |
| HTTP live streaming | (not available) |
| Current image (image/jpeg) | `https://dafang/cgi-bin/currentpic.cgi` |
| Get camera state | `https://dafang/cgi-bin/state.cgi?cmd=all` |
| Get system info | `https://dafang/cgi-bin/api.cgi?action=systeminfo` |

## What MQTT messages does the camera use?

### Sent by the camera

| MQTT topic | Comments |
| --- | --- |
| `<location>/<device_name>/`| General device status |
| `<location>/<device_name>/motion` | Motion detection status. Payload is either `ON` or `OFF`. |
| `<discovery_prefix>/<entity_type/<device_name>/<entity_name>/config` | Home Assistant MQTT auto discovery messages. More info: <https://www.home-assistant.io/docs/mqtt/discovery/> |
| `<location>/<device_name>/leds/blue` <br /> `<location>/<device_name>/leds/yellow` <br /> `<location>/<device_name>/leds/ir` <br /> `<location>/<device_name>/ir_cut` <br /> `<location>/<device_name>/rtsp_server` <br /> `<location>/<device_name>/night_mode` <br /> `<location>/<device_name>/night_mode/auto` <br /> `<location>/<device_name>/recording` <br /> `<location>/<device_name>/timelapse` | Camera configuration and status. Payload is either `ON` or `OFF` |
| `<location>/<device_name>/motion/detection` <br/ > `<location>/<device_name>/motion/led` <br/ > `<location>/<device_name>/motion/snapshot` <br/ > `<location>/<device_name>/motion/video` <br/ > `<location>/<device_name>/motion/mqtt_publish` <br/ > `<location>/<device_name>/motion/mqtt_snapshot` <br/ > `<location>/<device_name>/motion/send_mail` <br/ > `<location>/<device_name>/motion/send_telegram` <br/ > `<location>/<device_name>/motion/tracking` | Motion detection configuration. Payload is either `ON` or `OFF` |
| `<location>/<device_name>/motion/snapshot/image` | Sending an image via MQTT when motion is detected. |
| `<location>/<device_name>/motion/video` | Sending an video via MQTT when motion is detected. |
| `<location>/<device_name>/brightness` | Brigtness level, given as a percentage. Disabled by default. |
| `<location>/<device_name>/motors/vertical` <br/> `<location>/<device_name>/motors/horizontal` | Status of motors and alignment |

### Received by the camera

| MQTT topic | Comments |
| --- | --- |
| `<discovery_prefix>/set` | Trigger Home Assistant MQTT auto discovery announcement |
| `<location>/<device_name>/set status` | Get the device's system status |
| `<location>/<device_name>/play` | Play the audio file configured in the camera's settings |
| `<location>/<device_name>/leds/blue` | Get the device's system status |
| `<location>/<device_name>/timelapse/set` <br/> `<location>/<device_name>/recording/set` <br/> `<location>/<device_name>/rtsp_server/set` <br/> `<location>/<device_name>/rtsp_server/set` <br/> `<location>/<device_name>/night_mode/set` <br/> `<location>/<device_name>/night_mode/auto/set` <br/> `<location>/<device_name>/ir_cut/set` <br/> `<location>/<device_name>/leds/ir/set` <br/> `<location>/<device_name>/leds/yellow/set` <br/> `<location>/<device_name>/leds/blue/set` <br/> `<location>/<device_name>/motion/tracking/set` <br/> `<location>/<device_name>/motion/send_mail/set` <br/> `<location>/<device_name>/motion/detection/set` <br/> `<location>/<device_name>/motion/send_telegram/set` <br/> `<location>/<device_name>/motion/snapshot/set` <br/> `<location>/<device_name>/motion/video/set` <br/> `<location>/<device_name>/motion/mqtt_publish/set` <br/> `<location>/<device_name>/motion/mqtt_snapshot/set` <br/> `<location>/<device_name>/motion/mqtt_snapshot/set` <br/>  | Enable or disable a setting. Payload must be `ON` or `OFF`. |
| `<location>/<device_name>/snapshot/image GET` | Trigger a snapshot image to be sent over MQTT. |
| `<location>/<device_name>/motors/horizontal/set` | Rotate the camera. Payload must be either `left` or `right`. |
| `<location>/<device_name>/motors/vertical/set` | Rotate the camera. Payload must be either `up` or `down`. |
| `<location>/<device_name>/motors/set calibrate` | Re-calibrate the motors. |
| `<location>/<device_name>/remount_sdcard/set ON` | Remount the SD card. |
| `<location>/<device_name>/reboot/set ON` | Reboot the camera. |
| `<location>/<device_name>/update/set ON` | Update the camera's firmware. |
| `<location>/<device_name>/set (command)` | Invoke the command using `/system/sdcard/www/cgi-bin/action.cgi` |
