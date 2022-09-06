# Installing working openaps

- That repository installs openaps on ```da70837952034a9f23794eda7751701434d34508``` commit of ```dev``` branch.
- Original openaps: https://github.com/openaps/oref0

## Before you start:
- **Check your internet connection (```ping 8.8.8.8```) and DNS resolving (```ping google.com```)**. Otherwise try to [set Wi-Fi](https://github.com/workingopenaps/oref0/edit/dev/README.md#optional-set-wi-fi).
- **Save your preferences.json**
- Optional: save your autotune
- **Login into root user by ```sudo su -```**
- **Go to rig's home directory: ```cd /root```**

## 1) Remove two openaps folders:
1) ```rm -r src```
2) ```rm -r myopenaps```

## 2) Paste following commands:

1) ```curl https://raw.githubusercontent.com/workingopenaps/oref0/dev/bin/openaps-install.sh > /tmp/openaps-install.sh```
2) ```bash /tmp/openaps-install.sh```. When script will ask you ```Press Enter to run oref0-setup``` **press** **```CTRL-C```**

3) Paste this command: ```cd && ~/src/oref0/bin/oref0-setup.sh -dev```

4) Apply your preferences.json
5) Apply your autotune folder or just run ```cd /root/myopenaps && mkdir autotune``` to enable SMB

6) Check after 20 minutes, if you have **/root/test_data** directory. If you have, set autoremoving test_data folder.

## Optional: set autoremoving test_data folder

1) Go to crontab editing: ```crontab -e```
2) Paste ```* * * * * cd /root && rm -r test_data```
3) Save
4) Check the change by ```crontab -l```

## Optional: set Wi-Fi
### Intel Edison:

Paste this code into the terminal:
```
#!/bin/bash
(
dmesg -D
echo Scanning for wifi networks:
ifup wlan0
wpa_cli scan
echo -e "\nStrongest networks found:"
wpa_cli scan_res | sort -grk 3 | head | awk -F '\t' '{print $NF}' | uniq
set -e
echo -e /"\nWARNING: this script will back up and remove all of your current wifi configs."
read -p "Press Ctrl-C to cancel, or press Enter to continue:" -r
echo -e "\nNOTE: Spaces in your network name or password are ok. Do not add quotes."
read -p "Enter your network name: " -r
SSID=$REPLY
read -p "Enter your network password: " -r
PSK=$REPLY
cd /etc/network
cp interfaces interfaces.$(date +%s).bak
echo -e "auto lo\niface lo inet loopback\n\nauto usb0\niface usb0 inet static\n  address 10.11.12.13\n  netmask 255.255.255.0\n\nauto wlan0\niface wlan0 inet dhcp\n  wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf" > interfaces
echo -e "\n/etc/network/interfaces:\n"
cat interfaces
cd /etc/wpa_supplicant/
cp wpa_supplicant.conf wpa_supplicant.conf.$(date +%s).bak
echo -e "ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev\nupdate_config=1\nnetwork={\n  ssid=\"$SSID\"\n  psk=\"$PSK\"\n}" > wpa_supplicant.conf
echo -e "\n/etc/wpa_supplicant/wpa_supplicant.conf:\n"
cat wpa_supplicant.conf
echo -e "\nAttempting to bring up wlan0:\n"
ifdown wlan0; ifup wlan0
sleep 10
echo -ne "\nWifi SSID: "; iwgetid -r
sleep 5
)
```
### Raspberry Pi:

Follow the instructions of [Place your wifi and ssh configs on the new microSD card](https://openaps.readthedocs.io/en/latest/docs/Build%20Your%20Rig/pi-install.html#place-your-wifi-and-ssh-configs-on-the-new-microsd-card) section.
