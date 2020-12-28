# How to driver the Fibocom L850-GL on the Ubuntu 20.04 LTS

## 1\. install the xmm7360 linux driver

[Driver for Fibocom L850-GL / Intel XMM7360](https://github.com/xmm7360/xmm7360-pci "Driver for Fibocom L850-GL / Intel XMM7360 ")    <== download here

```shell
cd ./xmm7360-pci-master/
```

***
install the python liberary for need

```
sudo pip3 install --user ConfigArgParse
```
***
compile

```
make && make load
```

***
sudo python3 rpc/open_xdatachannel.py --apn websfrunlock the pin


```
echo "AT+CPIN=\"0000\"" | sudo tee -a /dev/ttyXMM1
```
***

enable the wwan0 -- APN ("internet" is APN for CHT )

```
sudo python3 rpc/open_xdatachannel.py --apn internet
```

* * *

check the wwan0 status

```
ifconfig -a
```

example
```
wwan0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet {mon IP}  netmask 0.0.0.0  destination {ma config réseau}
        inet6  {mon IP} prefixlen 64  scopeid 0x0<global>
        inet6 {mon IP} prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 1000  (UNSPEC)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
***

## 2\. Fix the WWAN0 DNS problem

```
nano /etc/netplan//etc/netplan
```

example

```
# Let NetworkManager manage all devices on this system
network:
    version: 2
    renderer: NetworkManager
    ethernets:
       wwan0:
          nameservers:
             addresses: [8.8.8.8, 8.8.4.4]
```

Apply the networking change
```
sudo netplan apply
```

## 3\. load shellscript sample

load4g.sh

```shell
#!/bin/bash
sudo insmod /home/alex/xmm7360/xmm7360-pci-master/xmm7360.ko
echo "AT+CPIN=\"0000\"" | sudo tee -a /dev/ttyXMM1
sudo python3 /home/alex/xmm7360/xmm7360-pci-master/rpc/open_xdatachannel.py --apn internetll
```



