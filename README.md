# Ubuntu 20.04 WiFi RaspberyPi 4B
How to configure Raspberry Pi 4B & Ubuntu 20.04 to use WiFi.
  This method uses cloud-init and is suitable for Raspberry Pi with a monitor or headless configuration.
  Tested using Raspberry Pi 4B /w 4GB of RAM & Ubuntu 20.04 LTS 64-bit.
  This solution has not been tested on any other board or OS versions.

NOTE: I first published this solution here: https://raspberrypi.stackexchange.com/a/113642/121286. 
Using Andbdrew's tips there for inspiration, I was able to make this work headless (and with a head) consistently without disabling Cloud-Init. My solution required a reboot because network-config changes are pulled in by cloud-init as it makes the netplan during first boot - before the regional wifi governning frequencies is instantiated. Therefore it required a reboot in order to get wlan0 up. Thanks to my friend, https://github.com/AWildBeard, for brainstorming with me on this problem - and for directing me to documentation for cloudinit: https://cloudinit.readthedocs.io/en/latest/topics/examples.html#reboot-poweroff-when-finished.

1. Image the SD Card using Raspberry Pi Imager v 1.3 selecting the 'UNBUNTU 20.04 LTS (RASBERRY PI 3/4)(64-bit) image.

2. When imaging is completed, remove the SD Card and then reinsert it so that it is mounted.

3. Copy 'network-config' & 'user-data' from the 'Files to Copy to SD Card' folder in this repository, overwitting the same two files on the SD Card.

If you choose to make your own version of these files by entering the changes manuauly intead, be especially careful with editors like Notepad++ that may enter tabs wherever you press 'enter' to start a new line. You must mind the indentations, use only two spaces for each indent, etc. as required by YAML and cloud-init.

4. Edit 'network-config' on the SD Card, replacing SSID and PassPhrase with your wireless access point credentials. Ensure the two values are wrapped in quotes. Save the modified file to the SD Card.

5. Eject the SD Card, remove from the computer used for imaging/editing and insert it into your powered-off Raspberry Pi 4B. Then, power up the Raspberry Pi.

6. Wait about 4 minutes to allow Ubuntu to boot, cloud-init to do its job, and then Ubuntu to reboot. If you observe with a montior, you will see the logon box appear after the first boot but it will take about two more minutes for cloud-init to show SSH keys, etc., and then reboot. If you try to logon before cloud-init is done the default credentials will not work as they are set by cloud-init.

7. Logon locally if you have a monitor and keyboard attached or if headless check your router, etc. to find the ip address and then connect using: ssh ubuntu@w.x.y.z.

That's it: Headless, Wi-Fi'd Ubuntu 20.04 on Raspbery Pi 4B.
