# RaspberryPiWifi

## About the project

Automatically configures all files needed for a raspberry pi wifi router

Based on the tutorial from https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md

Tested on a Raspberry Pi 3 with Raspian 8.0 (jessie)

## How to install

### Install additional software

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install dnsmasq hostapd
```

### Clone repository

```
git clone https://github.com/flmann/RaspberryPiWifi
```

All config changed can be made in the project folder. The script creates symbolic links to the config files in the system directories.

### Adjust config files

Configure ip address

in etc/dhcpcd.conf.link

```
interface eth0
static ip_address=141.56.000.000/24
static routers=141.56.000.000
static domain_name_servers=8.8.8.8
```

Configure ip address in subnet

in etc/dhcpcd.conf.link

```
interface wlan0
static ip_address=192.168.1.1/24
```

Configure dhcp ip range and lease time

in etc/dnsmasq.conf.link

```
dhcp-range=192.168.1.100,192.168.1.200,255.255.255.0,24h
```

Configure ssid and password

in etc/hostapd/hostapd.conf.link

```
interface=wlan0
driver=nl80211
ssid=networkname
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=password
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```

## Installing the config files

### What the script does:

The script scans all directories for files ending in _.link_ For all those a symbolic link will be created in the respective system directory. The project root will be kept relative to the system root. If you want to add an additional config file create the target system path at the project root and add _.link_ to the end of the file.

	Known Problem syntax highlighting will not work with _.link_ files

### Installation with install.sh

Execute the script

```
sudo install.sh
```

This installation only needs be executed once! Changes to the config files can be made any time, without installing again. However dnsmasq, hostapd or dhcpcd need to be restarted respectively.

After the installation a reboot is necessary:
```
sudo reboot
```
