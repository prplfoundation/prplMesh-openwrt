# OpenWrt packages for prplMesh

This feed contains packages for enabling WFA Multi-AP on a router.

This work was sponsored by [prpl Foundation](https://prplfoundation.org/).


## What is WFA Multi-AP?

WFA Multi-AP networks utilize multiple APs that work together to ensure all areas of the home have complete Wi-Fi coverage and adapt to changing network conditions. See the links below for details.

- https://www.wi-fi.org/file/wi-fi-certified-easymesh-technology-overview
- https://www.wi-fi.org/discover-wi-fi/specifications
- https://www.wi-fi.org/news-events/newsroom/wi-fi-certified-easymesh-delivers-intelligent-wi-fi-networks
- https://www.wi-fi.org/discover-wi-fi/wi-fi-easymesh
- https://www.wi-fi.org/file/infographic-wi-fi-certified-easymesh
- https://www.wi-fi.org/file/wi-fi-certified-easymesh-highlights


## How to add the prplMesh Feed to you OpenWrt/LEDE build

prplMesh requires a modified hostapd which is not yet in upstream OpenWRT. Therefore, you need to use multiap-upstream branch from https://git.openwrt.org/openwrt/staging/dangole.git

At the root of your OpenWrt/LEDE tree, add the following to your `feeds.conf` file:
```sh
src-git prplmesh https://github.com/prplfoundation/prplMesh-openwrt.git
```
Now to add the packages on your prplmesh feed to your OpenWrt/LEDE instance:
```sh
./scripts/feeds update prplmesh #retrieve the prplmesh feed from service/update to latest
./scripts/feeds install -p prplmesh #make all of the prplmesh feed packages available to the build
```

For more control over the package versions being installed, you can fork the feed using Github (and replace the `src-git` url) or maintaining a copy of the feed on your local system by using this line instead:
```sh
src-link prplmesh /full/path/to/feed/root
```
## Configuring the system

prplMesh must be configured with at least an AL MAC address. Configuration is done in the `prplmesh` config of UCI. This contains one section of type `prplmesh` that contains the
configuration of prplMesh itself, and additional sections of type `registrar` that contain the AP Autoconfiguration information for the controller.  The `prplMesh` section must contain an option `al_address` with the AL MAC address. 

To set up an unconfigured device that can be onboarded with push-button configuration, create an interface in UCI with the following configuration:
```json
{
	"encryption": "wps",
	"device": "radio0",
	"mode": "sta",
	"network": "lan",
	"wps_pushbutton": "1",
	"multi_ap": "1"
}

```

At the moment, prplMesh does not yet detect the new interface, so it needs to be restarted by hand, with `/etc/init.d/prplmesh restart`.

Push-button configuration can be started with:

```sh
ACTION=pressed BUTTON=wps /etc/rc.button/wps
```

prplMesh will then start to perform AP-Autoconfiguration over the new interface. It will get the AP information from the controller and configure the APs.

To configure a controller, add sections of type `registrar` to the UCI configurationo of prplMesh. This section must at least contain the option `ssid` with the SSID to configure.
It can also contain the option `key` with the encryption key, which sets WPA2-PSK on that SSID. Setting the option `backhaul` to 1 enables Multi-AP backhaul functionality on this AP. If also `backhaul_only` is set to 1, it is a backhaul-only AP, otherwise it is both backhaul and fronthaul. Finally, the bands can be selected with the `band` option: 1 for 2.4GHz, 2 for 5GHz, and 3 for both.

## Installing OpenWrt with Purple Mesh on Turris Omnia for the first time

### Materials

- Make sure you have a serial cable plugged into the 3 leftmost pins inside the Omnia next to the LED lights.  Left to Right: Black Yellow Orange

- TFTP Server available on your network

- An Ethernet cable from the host computer to any Omnia Lan Port

- An ethernet cable in the Omnia Wan Port to a DHCP server for the network containing the TFTP Server

- Files: 
  - omni-medkit-openwrt-mvebu-cortexa9-turris-omnia-initramfs.tar.gz 
  - openwrt-mvebu-cortexta9-turris-omnia-sysupgade.img.gz

### Setup

- Unzip the medkit file.  It will create ./boot/dtb and ./boot/zImage

- Place these files on your TFTP server 

- Plug your serial cable into the serial pins on the Omnia Left to Right: Black Yellow Orange
  - Note 4 pin should **not** be used.  

- Connect to the Omnia over serial

- *screen /dev/ttyUSB0 115200*  

## Ramdisk

- Reset the Omnia by holding in the reset until 4 lights are shown then release

- Omnia will reboot

- Interrupt uboot in the serial terminal console by hitting enter a few times

- Should get a => prompt

- Type `dhcp` 

- this should get an ip address from the dhcp server
  - *Note this will fail getting the tftp server because it’s not set*

```sh
=>setenv serverip <TFTP server IP address>
=>tftpboot 0x01000000 zImage
=>tftpboot 0x02000000 dtb
=>bootz 0x01000000 – 0x02000000
```

- Once the boot is done, from another terminal copy the sysupgrad to the Omnia

```bash
$scp openwrt-mvebu-cortexta9-turris-omnia-sysupgade.img.gz root@192.168.1.1:/tmp 
```

- Log into the Omnia through ssh and run sysupgrade

```shell
$ssh root@192.168.1.1
=>sysupgrade /tmp/openwrt-mvebu-cortexta9-turris-omnia-sysupgade.img.gz

```

- This should chug and churn for a while with lights flashing.  If all goes well, the Omnia will reboot itself and you should see on the serial console

> OpenWRT. Wireless Freedom. OpenWrt SNAPSHOT r8703-e056a9cdbe (or some such)

### Possible Issues

- If the Omnia gets in a weird state (e.g. can’t find kernel) reset the environment

```sh
env default -a
saveenv
```

- If all else fails, try reseting by holding in the reset until 4 lights are shown then release

