# Net-Mesh-Forest

Our mission is to implement solutions dedicated to the conservation of wildlife and to ensure the survival of these species. It is also important to ensure our implementation is effective and cost efficient. 

### Disclaimer:

Use of this tool for malicious purposes is not recommended. Author(s) is/are not responsible or liable for any damage or actions to any device compromised. It is intended for educational purpose only.

## Whats Needed:

Wyzecam V2
2 x MicroSD Cards

## Attention: Do not install the latest Firmware on your Device. This hack will not be possible.

### How to install Custom Firmware(CFW)

The CFW consists of two parts:

1. The Custom Firmware, which doesn't contain any custom software, it just allows you to boot from a Micro SD card. This will only need to be done once. It alters the stock firmware to give you the ability to boot from a Micro SD card and needs to be flashed over the stock firmware.
2. The CFW Files, which contains the custom software. You will have to install them onto your Micro SD card after you complete step 1. You can easily modify the software by editing the files on the Micro SD card.







## Which Features does the CFW contain?

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
