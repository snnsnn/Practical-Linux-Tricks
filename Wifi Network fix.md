# Check if wifi power saving is turn on:

```bash
$ iwconfig
```

If so turn it off by changing powersave flag to 2 or adding following lines to `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf`:

```
[connection]
wifi.powersave = 2
```

# Fix driver settings

The driver that your device uses can be problematic but sometimes if we add some parameters to it and change some settings in the router we can get it to work if not we will have to compile an new driver.

* First in your router change 802.11bgn to 802.11bg.

* Second change the wep encryption to just wpa2 (CCMP)(AES) not (TKIP) if you have that option it will work best.

* Third set your wireless channel in the router to 1 or 11 then save the router configuration and reboot it.

* Fourth go into network manager at top right corner of the screen and click on edit connections>wireless tab and set IPV6 to ignore.

Now open the terminal and paste the following code one line at a time for accuracy:

echo "options rtl8192ce swenc=1 ips=0 fwlps=0 swlps=0 ant_sel=2" | sudo tee /etc/modprobe.d/rtl8192ce.conf
sudo modprobe -rfv rtl8192ce
sudo modprobe -v rtl8192ce

To get the meaning of these flags run this command:

```bash
modinfo rtl8192ce
```

Basically:
* swenc:Set to 1 for software crypto (default 0)
* ips:Set to 0 to not use link power save (default 1)
* swlps:Set to 1 to use SW control power save (default 0)
* fwlps:Set to 1 to use FW control power save (default 1)
* aspm:Set to 1 to enable ASPM (default 1)
* debug_level:Set debug level (0-5) (default 0) (int)
* debug_mask:Set debug mask (default 0) (ullong)

# Set anthenna value to increase signal strength

Above code already set anthenna value to ant_sel=2 to boost the signal, but this needs to be checked first if required:

Where, ant_sel can be 0, 1 and 2 for default, AUX and MAIN antenna position respectively. Note: Once ant_sel is changed, you have to do a cold reboot. The power should be off to ensure that the firmware is using the new value.

After rebooting, check if parameter is correct. The easiest test is to run the command

```bash
sudo iw dev wlan0 scan | egrep "SSID|signal"
```

or 
```
$ DEVICE=$(iw dev | grep Interface | cut -d " " -f2)
$ sudo iw dev $DEVICE scan | egrep "SSID|signal|\(on"
```

If your device is named something other than wlan0, use that name. If the signal value for your AP is -60 dBm or lower, then this antenna selection is probably wrong. With values this small, connection will be difficult.

After doing three experiments with ant_sel=0, 1, 2, you can write the best one to rtl8723be.conf.

Note: Most laptops have two antennas, and the 3 values will be nearly equal. This test is most important for those computers that have only one antenna, AND an incorrectly encoded EFUSE.

# Turn off Windows 10 fastboot (in Windows energy settings)

# Bios -> Boot Option (Enter) -> Legacy Support: Disabled
