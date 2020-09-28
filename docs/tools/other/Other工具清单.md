
## Other-tool

### scrcpy
How to run scrcpy wirelessly?
Here are the steps:
1. Connect the device to the same Wi-Fi as your computer
2. Get your device IP address (in Settings → About phone → Status)
3. Enable adb over TCP/IP on your device: `adb tcpip 5555`
4. Connect to your device: `adb connect DEVICE_IP:5555` (replace `DEVICE_IP`)
5. Unplug your device
6. Run `scrcpy` as usual

To switch back to USB mode: `adb usb`.
As expected, the performances are not the same as over USB.
The default scrcpy bit-rate is 8Mbps, which is probably too much for a Wi-Fi connection. Depending on the use case, decreasing the bit-rate and the resolution may be a good compromise:
`scrcpy --bit-rate 2M --max-size 800`
For people in a hurry:
`scrcpy -b2M -m800`
