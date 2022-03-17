# Diluate your traffic and escape mass surveillance


Here is a free and open source solution I've found to improve your privacy one step at a time while browsing the internet.


To cover your tracks online, it could be wiser to generate some white noise to diluate your data than consider funneling completely your entire network connection into Tor or some public VPN services.

{{< figure src="/images/tutorials/mass-surveillance/diluate-your-traffic-and-escape-mass-surveillance0.png" alt="a cool picture about eavesdropping" >}} 
_A frame capture showing how Tor is actually like a bull in a China shop._

The idea here is to protect yourself from mass surveillance by generating some useless or inaccurate traffic which makes your profiling imprecise and thus ineffective.

## Installation

*Whisperer* is a simple program written in Go under the MIT open source license. As its author describes it best, it makes HTTP request in background with some random delay between requests. This allows you to generate realistic random HTTP/DNS traffic noise. 

You can install it on your local machine but I recommend you to run it on a local server (Raspberry PI for instance or NAS). This way, it will generates white noise all the time. 

These instructions will get you a copy of the project up and running on your machine. 

First, clone the repository : 
```sh
$ git clone https://github.com/danielkvist/whisperer
```
Then navigate into the *Whisperer* directory :
```sh
$ cd whisperer
```
Finally run :
```sh
$ go run main.go
```

As you can see, *Whisperer* can accept a number of command line arguments:

```sh
$ whisperer --help
whisperer makes HTTP request constantly in order to 
generate random HTTP/DNS traffic noise.

Usage:
  whisperer [flags]

Flags:
  -a, --agent string       user agent
      --debug              prints error messages
  -d, --delay duration     delay between requests
  -g, --goroutines int     number of goroutines
  -h, --help               help for whisperer
  -p, --proxy string       proxy URL
  -r, --random             random delay between requests
  -t, --timeout duration   max time to wait for a response before canceling the request
      --urls string        simple .txt file with URLs to visit
  -v, --verbose            enables verbose mode
```

## Using docker

To use *Whisperer* as a Docker container you can pull the image with the following command :

```sh
$ docker image pull danielkvist/whisperer
```
_Note that the image danielkvist/whisperer uses the urls file from the repository. So it is not a valid option if you want to customize the URLs that Whisperer is going to visit._

First, clone the repository :
```sh
$ git clone https://github.com/danielkvist/whisperer
```

Then navigate into the whisperer directory :
```sh
$ cd whisperer
```

Modify the urls.txt file if you want :
```sh
$ vim urls.txt
```

Build the Docker Image from the Dockerfile inside the repository :
```sh
$ docker image build -t whisperer .
```

Finally Run :

```sh
$ docker container run whisperer
```

[Here](https://github.com/danielkvist/whisperer) is the Github repository for reference.

## Make a good action !
---
Hit that `CTRL + D` to bookmark this website 🔖

Internet is for sharing, so please consider sharing this post if you liked it !

It will be so much better than any newsletter #privacy #spam #planet 🌍 ❤️
