# Ubuntu 20.04 WiFi RaspberyPi 4B
Configuring Ubuntu 20.04 to use WiFi on Raspberry Pi 4B using cloud-init (for use with a monitor or headless).
NOTE: I first published this solution here: https://raspberrypi.stackexchange.com/a/113642/121286. 
Using Andbdrew's tips there, I was able to make this work headless (and with a head) consistently without disabling Cloud-Init. Thanks to @enconn, for brainstorming with me on this, and for directing me to documentation for cloudinit: https://cloudinit.readthedocs.io/en/latest/topics/examples.html#reboot-poweroff-when-finished.

1. Image the SSD using Raspberry Pi Imager v 1.3 selecting the 'UNBUNTU 20.04 LTS (RASBERRY PI 3/4)(64-bit) image.

2. When imaging is completed, remove the SSD and then reinsert it so that it is mounted.

3. Modify 'network-config' found on the SSD so that it contains only the items in the example below. Comment out with #, or remove, all other settings including the LAN ones. Enter any missing items. Be certain to maintain only the indentations shown. Use two spaces for each indentation. Remove all tab characters. Be especially careful if using Notepad++ as it will enter tabs wherever enter is used to start a new line. Replace the SSID with your wireless SSID and the PassPhrase with your wireless passphrase. When done, those two values should be wrapped in quotes. Save the modified file to the SSD.

4. Edit 'user-data' appending the additional lines shown below. Again, use spaces, not tabs and mind the indentation.

5. Eject the SDD, remove it from the computer used to image it, and then insert into the powered off Raspberry Pi. Then, power up the RPi.

6. Allow Ubuntu to boot; DO NOT try to log into Ubuntu as soon as possible. Wait until Cloud-Init runs (although it appears to be doing nothing - in about two minutes it will show SSH info when done). If you don't wait you may not be able to logon with the default user and passwd. At the end of the cloud-init,Ubuntu will be rebooted. What a couple of minutes for the server to boot.

7. Logon either at the console or remotely using ssh ubuntu@x.x.x.x (get the address from your router, etc.)

That's it: Headless, Wi-Fi'd Ubuntu 20.04 on Raspbery Pi 4b

network-config example:

# This file contains a netplan-compatible configuration which cloud-init
# will apply on first-boot. Please refer to the cloud-init documentation and
# the netplan reference for full details:
#
# https://cloudinit.readthedocs.io/
# https://netplan.io/reference
#

version: 2
renderer: networkd
wifis:
  wlan0:
    dhcp4: true
    dhcp6: true
    optional: true
    access-points:
      "SSID":
         password: "PassPhrase"

Append this to the end of 'user-data':

##Reboot after cloud-init completes
power_state:
  mode: reboot
