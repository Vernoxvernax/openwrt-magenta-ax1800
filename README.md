![OpenWrt logo](include/logo.png)

# What the?

This is a fork to provide rudimentary support for the "T-Mobile/Magenta 5G Box Outdoor Inside - AX1800". Aside from the terrible name it's also running a terribly outdated and heavily worsened version of Lede (OpenWrt 1000 years ago). Usually newly supported devices can be easily integrated into the OpenWrt buildsystem, but unfortunately Magenta (probably) made WNC (manufacturer) change magic numbers among other crimes to (perhaps) avoid situations where people can just flash their own OS on it. (*they like data*, literally insane what information they were (mayhaps) uploading through easycwmp).

Either way, it's been like 11 months of work trying to make all the pieces fit to get this working. I have a lot of people to thank in the community, but it should also be mentioned that Magenta themselves were willing to opensource their ancient buildsystem upon my request.

Currently, this project exists only because of my own interest, which is also why only the minimum actually works.

Things I haven't bothered with:

- flashing firmware without tftp in uboot
- LEDs (only the power is working)
- more than 20mb of usable space
- the USB port
- reading the ethernet mac address (they'll be random, but you can assign them manually, which you should)

WiFi 6 (+ Wifi MAC addresses), Ethernet (LAN & WAN) are all working as of right now.


# Setup

To install this, get the image build using the openwrt build guide on their wikipage [here](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem).

Then you have to connect to the UART on the device. ![where my mouse cursor points at](https://i.imgur.com/Iiw7Iek.png)

From left to right: Pin 2 is TX and Pin 4 is RX.

During boot up you'll get a small menu with a 5 second timer.
Just push the up or down key to cancel the timer.

Connect your computer to the WAN (yellow) port of the device.
We'll be using that computer to host the firmware file with tftp.
Since uboot doesn't have DHCP and because we are also connected to the lan port we have to assign an ip-address to the NIC on the computer. Like 192.168.2.254 - you won't be able to ping the router just yet. Then install a tftp server of your liking and host the firmware file on it.

Choose the second option "System Load Linux Kernel then write to Flash via TFTP."
The script will ask you what the name of the firmware file is and the ip addresses. If all goes well you should see the file being transfered and then flashed to the NAND (oh yea, did I mentioned you can't go back?).
Then you'll be dropped into the uboot shell from where you can launch OpenWRT using `run boot2`.
You may need to change the bootargs in uboot to `mtdparts=slave` or `mtdparts=master` depending on where OpenWRT is written to.

WiFi is be disabled by default so you'll need to connect to one of the black RJ-45s and configure it however you like. Have fun.

Notes: 25.12 is the first version that should theoretically add sysupgrade support. So you should be able to just flash newer versions via LuCI or over SSH. Note that it will always complain about the invalid magic number, which you can always bypass by just forcing the upgrade.

If there are any problems or you want me to get anything additional working just create an issue.

Stuff:
https://forum.openwrt.org/t/add-support-for-t-mobile-mtk7622-5g-idu

Thank you evs.

___

## License

OpenWrt is licensed under GPL-2.0
