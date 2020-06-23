# Ubuntu 20.04 Wi-Fi RaspberyPi 4B
How to configure Raspberry Pi 4B & Ubuntu 20.04 to use Wi-Fi:
 
1. Image the SD Card using Raspberry Pi Imager v 1.3 selecting the 'UNBUNTU 20.04 LTS (RASBERRY PI 3/4)(64-bit) image.

2. When imaging is completed, remove the SD Card and then reinsert it so that it is mounted.

3. Copy 'network-config' & 'user-data' from the 'Files to Copy to SD Card' folder in this repository, overwitting the same two files on the SD Card.

4. Edit 'network-config' on the SD Card, replacing SSID and PassPhrase with your wireless access point credentials. Ensure the two values remain wrapped in quotes. Save the modified file to the SD Card.

5. Eject the SD Card, remove from the computer used for imaging/editing and insert it into your powered-off Raspberry Pi 4B. Then, power up the Raspberry Pi.

6. Wait about 4 minutes to allow Ubuntu to boot, cloud-init to do its job, and then Ubuntu to reboot. If you observe with a montior, you will see the logon box appear after the first boot but it will take about two more minutes for cloud-init to show SSH keys, etc., and then reboot. If you try to logon before cloud-init is done the default credentials will not work as they are set by cloud-init.

7. Logon locally if you have a monitor and keyboard attached or via SSH if headless. See Ubuntu Tutorial: https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#4-boot-ubuntu-server

That's it: Headless, Wi-Fi'd Ubuntu 20.04 on Raspbery Pi 4B.

Additional Notes
If you choose to make your own version of 'network-config' and/or 'user-data' rather than copy mine, be especially careful with editors such as Notepad++, as they may enter tab characters when you press 'enter' to start a new line. Tab charcters are not valid for the files, as they are consumed by cloud-init to build the netplan YAML files. Use two spaces for indentations. You must maintain the indentations for the file to be interpreted correctly.

My first google search to solve this problem ran into the thread https://raspberrypi.stackexchange.com/a/113642/121286. I didn't care for the solutions there that called to disabled cloud-init but drew inspiration from Andbdrew's tips and was able to make this work headless (and with a head) consistently without disabling cloud-init. To make this work, I eliminated all interfaces from 'network-config' except wlan0 and set 'renderer: networkd'. It appears the image does not contain a full implimentation to support 'renderer: NetowrkManager' - which I hoped would eliminate the 'needs a reboot to work' problem I encountered. It appears it may be that cloud-init configuraiton, which appears on screen about 2 minutes after the logon problem, occurs too late in the boot cycle for the regional db containing wifi frequencies to be applied - and thus a reboot after cloud-init is necessary. Thanks to my friend, https://github.com/AWildBeard, for brainstorming with me on this problem - and for directing me to documentation for cloudinit: https://cloudinit.readthedocs.io/en/latest/topics/examples.html#reboot-poweroff-when-finished.

REF: Cloud-init documentation: https://cloudinit.readthedocs.io/en/latest
