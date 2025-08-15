# rtw88
===========
### A repo for the newest Realtek rtlwifi codes.

This code will build on any kernel 4.19 and newer as long as the distro has not modified
any of the kernel APIs. IF YOU RUN UBUNTU, YOU CAN BE ASSURED THAT THE APIs HAVE CHANGED.
NO, I WILL NOT MODIFY THE SOURCE FOR YOU. YOU ARE ON YOUR OWN!!!!!

This repository includes drivers for the following cards:

RTL8723DE, RTL8723DU, RTL8821CE, RTL8821CU, RTL8822BE, RTL8822BU, RTL8822CE and RTL8822CU

If you are looking for a driver for chips such as 
RTL8188EE, RTL8192CE, RTL8192CU, RTL8192DE, RTL8192EE, RTL8192SE, RTL8723AE, RTL8723BE, or RTL8821AE,
these should be provided by your kernel. If not, then you should go to the Backports Project
(https://backports.wiki.kernel.org/index.php/Main_Page) to obtain the necessary code.

This repo has been brought up to date with the kernel code on Sep. 25, 2020.

The main changes are as follows:
1. The methods for obtaining DMA buffers has changed. This should have no effect.
2. The regulatory methods are changed. This may have some effect on users.
3. The firmware loading has been more resistent against timeouts.
4. The RX buffer size is increased.
5. Antenna selection code was modified. This change may help the low signal problems.
6. BlueTooth coexistence was modified.

When making these changes, I tried to watch for things that might be incompatible
with older kernels. As this kind of updating in really boring, I might have missed
something. Please let me know of build problems.

## Build

```console
$ make clean
$ make
```

## load drivers, preferred

```console
$ sudo make load
```

## install drivers
did you have run make load ?

```console
$ sudo make install
```

## install firmware

```console
$ sudo make firmware
```

## NOTE
This driver will naturally clash with upstream rtw88 drivers  
For PCI based device you need the drivers from this location  


## General Commands

Scan:
```console
$ sudo iw wlanX scan
```
Connect to the AP without security:
```console
$ sudo iw wlanX connect <AP name>
```
## Wifi Sniffer - monitor mode
```console
$ sudo ip link set wlanX down
$ sudo iw dev wlanX set type monitor
$ sudo rfkill unblock all
$ sudo ip link set wlanX up
```

Then you can use "iw <device> info" to check if the wireless mode is correct.
```console
e.g.
    wlan1    IEEE 802.11  Mode:Monitor ... 
```

And you can use the program like wireshark to sniffer wifi packets.
1. set up the sniffer channel
```console
$ sudo iw dev wlanX set channel xxx
```

2. run the program
```console
$ sudo wireshark
```

## Test
test ok with general commands with the latest kernel
ubuntu 18 + kernel v5.3 test with Network Manager ok. 

## Known Issues

* Currently, this driver is not upstreamed to Linux kernel driver rtw88 yet. That means, loading this module will cause unpredictable results to other working Realtek wifi pcie device, especially to those laptops with Realtek wifi IC running kernel > v5.2.
