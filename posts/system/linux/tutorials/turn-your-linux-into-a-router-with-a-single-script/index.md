# Turn your Linux into a router with a single script


Want to turn your GNU/Linux machine into a powerful router able to connect to the Internet, to provide you a Wifi hotspot, to act as a transparent proxy or simply to properly route your virtual machines / containers ports ?


I have some good news for you then ! If you have an old GNU/Linux lying around (e.g. an unused Raspberry Pi), you can easily turn it into a router thanks to Linux-router (lnxrouter).

This tool is a script wich calls iptables, dnsmasq or even hostapd to :

- Create a NAT subnet
- Provide Internet access
- Provide a DHCP server and Relay Agent
- Provide a DNS server
- Have IPv6 (behind the NAT LAN, just like IPv4)
- Create a wifi hotspot:
    - Select the wifi channel as well as its encryption WPA2/WPA, WPA2, WPA (no WPA3 supported yet)
    - Create an access point on the same interface as the one where you get Internet.
- Set up a transparent proxy (redsocks)
- To set up a DNS proxy

Linux Router can be extremely useful in the following network configurations :

```
Internet----(eth0/wlan0)-Linux-(wlanX)AP
                                       |--client
                                       |--client
```

```
                                    Internet
WiFi AP(no DHCP)                        |
    |----(wlan1)-Linux-(eth0/wlan0)------
    |           (DHCP)
    |--client
    |--client
```

```
                                    Internet
 Switch                                 |
    |---(eth1)-Linux-(eth0/wlan0)--------
    |--client
    |--client
```

```
Internet----(eth0/wlan0)-Linux-(eth1)------Another PC
```

```
Internet----(eth0/wlan0)-Linux-(virtual interface)-----VM/container
```

## Some examples
---##### Wi-Fi Hotspot

To create a Wi-Fi hotspot, you only have to enter the following command : 

`sudo lnxrouter --ap wlan0 AccessPoint -p Password`
### Tor transparent proxy

To set up a transparent proxy that goes through Tor : 

`sudo lnxrouter -i eth1 --tp 9040 --dns 9053 -g 192.168.55.1 --p6 fd00:5:6:7::`

And specify the following config in the torrc file : 

```
TransPort 192.168.55.1:9040 
DNSPort 192.168.55.1:9053
TransPort [fd00:5:6:7::1]:9040 
DNSPort [fd00:5:6:7::1]:9053
```
### VirtualBox transparent proxy

Or if you only want to create a transparent proxy for your VirtualBox VMs :

`sudo lnxrouter -i vboxnet5 --tp 9040 --dns 9053`

I won't explain all the possibilities of the software, but in concrete terms, lnxrouter doesn't do anything more than what it is possible to do with a basic Linux. It's just that this script makes things easier and quicker. I'm sure you'll like it.

Of course, you can find the script as well as the documentation on [GitHub](https://github.com/garywill/linux-router).

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
