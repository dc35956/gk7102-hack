# Hack for Cheap Chinese Goke GK7102 Cloud IP Cameras

This hack is for various Chinese Goke GK7102 based IP Cameras. There are few different firmware varieties across various brands of cameras, which means it is impossible to know what exactly will work for you. Older firmware are more hackable because the root filesystem is mounted read/write. In the new firmwares the root filesystem is mounted read-only but the /home directory is writeable. This hack is based on (https://github.com/ant-thomas/zsgx1hacks)


## Working on the following camera models

* V-Tac 1080P IP Indoor Camera (read-only version)
* ZS-GX1
* Snowman SRC-001
* GUUDGO GD-SC01
* GUUDGO GD-SC03
* GUUDGO GD-SC11
* Digoo DG-W01F
* Digoo DG-MYQ
* YSA CIPC-GC13H
* KERUI CIPC-GC15HE (read-only version)
* EYEPLUS CIPC-GC13H (read-only version)
* HIP291-1M
* 2M-AI
* others on the GOKE processor: GK7102 (GK7102S)


## Features

* Blocking cloud hosts
* New version of BusyBox v1.26.2
* Configurable settings
* BusyBox FTP Server
* dropbear SSH Server: root can login ssh without password
* WebUI PTZ - (http://192.168.200.1:8080/cgi-bin/webui)
* Debug and diagnostics tools: [andrew-d/static-binaries](https://github.com/andrew-d/static-binaries/tree/master/binaries/linux/arm)
* Change login credentials to ```user: root password: cxlinux```
* Improved terminal experience
* Wi-Fi configuration without cloud account


## Installation

Current version works only from microSD card and do not require installation. It should work normaly on both old and new read-only firmwares.

* Download the hack
* Copy contents of folder ```sdcard``` to the main directory of a vfat/fat32 formatted microSD card
* Change options in ```config.txt```
* Insert microSD card into camera and reboot the device
* Enjoy


## Security

The security of these devices is terrible.
* DO NOT expose these cameras to the internet.
* config.txt is used to decide what servers to run.
* This hack is blocking the communication with the cloud providers, see /media/hack/etc/hosts.
* The device was communicating with servers of icloseli.cn, icloseli.com and arcsoft.com
* Some cameras are trying to talk with 30.108.91.227, even with the blocked hosts.
* tcpdump binary is included with this hack on /media/hack/bin/tcpdump
* DO NOT expose these cameras to the internet.


## Instructions

### How to check version

* Using an onvif tool/app like Onvifer (Android) should give firmware version.
* You should also be able to find the firmware version by logging in via telnet and excuting the command
* If you have already configured the camera with the cloud app there should be some info within the app showing firmware version.
```
ls /tmp | grep -F 3. or ls /tmp | head -1
```

### Default Credentials

   IP                Port     Service     Username     Password     
   ------            ----     -------     --------     --------     
   192.168.200.1     23       telnet      root         cxlinux     


### RTSP Connection

* rtsp://admin:@192.168.200.1:554
* rtsp://admin:@192.168.200.1:8001


### Debug Scripts and Files

By default, the startup script ```/home/start.sh``` will try to load and run some commands

#### Files running from SD Card

To run debug scripts create a file ```debug_cmd.sh``` on an SD card and you will be able to execute bash commands from it.

#### Files copied from SD Card to /home

```*-hwcfg.ini, *-VOICE.tgz, *-ptz.cfg, *-hardinfo.bin, *-custom_init.sh```

#### File will be flashed

```firmware.bin```


### Connect to Wi-fi network

On the SD card, create the ```cls.conf``` file and write these 3 lines in it. Change your_wifi_ssid and your_wifi_password with your credentials.
```
[cls_server]
ssid = your_wifi_ssid
passwd = your_wifi_password
```

### Tweaks

* [Security, RTSP, SSH, Video files to SD Card](https://github.com/ant-thomas/zsgx1hacks/pull/93/files)
* [Logs and other useful ideas](https://github.com/ant-thomas/zsgx1hacks/pull/4/files)
* [Original ant-thomas with Persistent hack](https://github.com/ant-thomas/zsgx1hacks)
* [Read-only modular customizer by bolshevik](https://github.com/bolshevik/goke-GK7102-customizer)



## Device Details

### Product

1080P IP Indoor Camera EU Power Plug & Auto Track Function
Brand: V-Tac
Model: VT-5122

[V-Tac 1080P IP Indoor Camera ](https://www.v-tac.eu/products/smart-home/cameras/1080p-ip-indoor-camera-eu-power-plug-auto-track-function-detail.html)


### Wi-Fi Manufacturer

MAC Address: 88:E6:28:xx:xx:xx - Shenzhen Kezhonglong Optoelectronic Technology Co.,Ltd
Floor 3, Bldg. 5, Area B, Xinfu Industrial Park, Chongqing Rd., Baoan Dist,Shenzhen,Guangdong, China Shenzhen Guangdong CN 518103
(https://fccid.io/2AF2K-WM415)


### Software Versions
```
$ ls /tmp | grep -F 3.
3.4.2.0724
gc2033

$ uname -a
Linux localhost 3.4.43-gk #46 PREEMPT Wed Jun 26 13:47:34 CST 2019 armv6l GNU/Linux

$ busybox
BusyBox v1.22.1 (2019-06-18 19:35:31 CST) multi-call binary.

$ cat /etc/issue
Welcome to Goke Linux

p2pcam
ver: 3.4.2.0724
Get Hardware Info: model:Y13,  firmware-ident:eyeplus_ipc_gk_008

"GOKE 7102C BOARD"
```

### Hardware info
```
$ cat /home/hardinfo.bin
<DeviceClass>0</DeviceClass>
<OemCode>0</OemCode>
<BoardType>1007</BoardType>
<FirmwareIdent>eyeplus_ipc_gk_008</FirmwareIdent>
<Manufacturer>RS</Manufacturer>
<Model>GK7102</Model>
<GPIO>
<BoardReset>53_0x00000000_0_0</BoardReset>
<SpeakerCtrl>33_0x00000000_0_0</SpeakerCtrl>
<WhiteLight>15_0x00000000_0_1</WhiteLight>
<IrCtrl>12_0x00000000_0_1</IrCtrl>
<IrCut1B>44_0x00000000_0_1</IrCut1B>
<IrCut2B>39_0x00000000_0_1</IrCut2B>
</GPIO>
```

### Open ports
```
$ nmap -sV 192.168.200.1
Nmap scan report for _gateway (192.168.200.1)
Host is up (0.015s latency).
Not shown: 993 closed ports
PORT     STATE SERVICE    VERSION
23/tcp   open  telnet     BusyBox telnetd
80/tcp   open  http       Ginatex-HTTPServer
554/tcp  open  rtsp
843/tcp  open  unknown
5050/tcp open  mmcc?
7103/tcp open  tcpwrapped
8001/tcp open  rtsp
4 services unrecognized despite returning data.
MAC Address: 88:E6:28:xx:xx:xx (Shenzhen Kezhonglong Optoelectronic Technology)
```

### Processor
```
$ cat /proc/cpuinfo 
Processor       : ARMv6-compatible processor rev 7 (v6l)
BogoMIPS        : 597.60
Features        : swp half fastmult vfp edsp java tls 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xb76
CPU revision    : 7

Hardware        : Goke IPC Board
Revision        : 0000
Serial          : 0000000000000000
$ 
```

### Memory
```
$ cat /proc/meminfo 
MemTotal:          31844 kB
MemFree:            2192 kB
Buffers:            2284 kB
Cached:            11256 kB
SwapCached:            0 kB
Active:             5488 kB
Inactive:          11856 kB
Active(anon):       3996 kB
Inactive(anon):      536 kB
Active(file):       1492 kB
Inactive(file):    11320 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:          3820 kB
Mapped:             3412 kB
Shmem:               728 kB
Slab:               8548 kB
SReclaimable:       1212 kB
SUnreclaim:         7336 kB
KernelStack:         656 kB
PageTables:          384 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:       15920 kB
Committed_AS:     163912 kB
VmallocTotal:    2039808 kB
VmallocUsed:       63408 kB
VmallocChunk:    1005336 kB
```

### /etc/passwd
```
$ cat /etc/passwd
root:yE7gW4O0CSXXg:0:0::/root:/bin/sh
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:100:sync:/bin:/bin/sync
mail:x:8:8:mail:/var/spool/mail:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
operator:x:37:37:Operator:/var:/bin/sh
haldaemon:x:68:68:hald:/:/bin/sh
dbus:x:81:81:dbus:/var/run/dbus:/bin/sh
ftp:x:83:83:ftp:/home/ftp:/bin/sh
nobody:x:99:99:nobody:/home:/bin/sh
sshd:x:103:99:Operator:/var:/bin/sh
default:x:1000:1000:Default non-root user:/home/default:/bin/sh
```

### cat /home/cloud.ini 
```
[SERVERINFO]
server_name=arcsoft.com
xmpp_server_ip=xmpp.icloseli.cn.
relay_server_ip=relaycn.arcsoftcloud.com
auto_update_server_ip=update.icloseli.cn.
lecam_purchase_server_ip=esd.icloseli.cn.
upns_pnserver=upns.icloseli.cn.
upns_xmpp_name=arcsoft.com
upns_xmpp_ip=xmpp.icloseli.cn.
argus_api_server_ip=argus.icloseli.cn.
argus_server_ip=argus.icloseli.cn.
relay_server_domain_name=relay.icloseli.cn.
stun_server_ip=stun.icloseli.cn.
cloud_auth_server_name=api.icloseli.cn.
bell_server_ip=bell.icloseli.cn
return_server_ip=relaycn.closeli.cn
```


## References

* [V-Tac VT-5122: 1080P IP Indoor Camera EU Power Plug & Auto Track Function](https://www.v-tac.eu/products/smart-home/cameras/1080p-ip-indoor-camera-eu-power-plug-auto-track-function-detail.html)

* [Hacks for ZS-GX1 IP Camera and various Goke GK7102 based IP Cameras](https://github.com/ant-thomas/zsgx1hacks)

* [The BusyBox project](https://busybox.net/)

* [ARM Binary andrew-d/static-binaries](https://github.com/andrew-d/static-binaries/tree/master/binaries/linux/arm)

* [WiFi IP CloudCamer-ы: HIP291-1M/2M-AI, Digoo DG-W01F, Digoo DG-MYQ и другие на процессоре от GOKE: GK7102 (GK7102S)](https://4pda.ru/forum/index.php?showtopic=928641)

* [GOKE HD IP CAMERA SOLUTION GK7101 GK7102](https://www.unifore.net/company-highlights/goke-hd-ip-camera-solution-gk7101-gk7102.html)

* [Aliexpress: INQMEGA 1080P Cloud Wireless IP Camera Intelligent Auto Tracking](https://www.aliexpress.com/item/32796421899.html)

* [threat9/routersploit - Exploitation Framework for Embedded Devices](https://github.com/threat9/routersploit)

* [GK7102 Based IP Camera](https://gist.github.com/brianpow/d8eeaee0879b1fd46ccedfae04799f49)

* [Read-only modular GOKE GK7102/GK7102S camera customizer](https://github.com/bolshevik/goke-GK7102-customizer)
