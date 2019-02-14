---
layout: post
title:  "Getting up and running with archlabs on a lenovo T480"
date:   2019-02-13 18:50:46 -0400
categories: jekyll update
---

**Overview**
For the most part these are some fairly sparse notes about installing archlinux on a lenovo thinkpad T480. I bought mine without the built in Nvidia graphics card to avoid the headache of video drivers. Unfortunately this post will be unhelpful to you if you are looking for driver help.

A few months ago ago I was fortunate enough to be able to update my laptop to a Lenovo Thinkpad T480. This was a machine that I had bought specifically for the purpose of installing linux on it, as opposed to my previous old mac laptop which had numerous difficulties with running Arch linux out of the box.

**Bootup**
One thing of note about installing linux on a new T480, it does require that you allow boot from alternate sources and undo some of the "safety features" in the boot menu. To get to this boot menu press f2 as the computer boots up.
However, after this setup was for the most part pretty straight forward. 

**Installation**
I opted to use a lightweight arch linux installer to walk me through some of the more tedious parts of an arch linux installation, and for the most part I was pleasantly surprised.

If you are someone who prefers a very custom and or light setup but doesn't want to worry too much about the hassle of installing an arch distribution from scratch, I would highly recommend archlabs. They have support for i3, and some pretty sane defaults for most of the settings that you would want, [here][archlabs] is some more information about them.

**Internet Connections**
Wifi worked for me _almost_ out of the box so no major issues here.

NetworkManager and netctl sometimes seemed like they were competing as NetworkManager was installed by archlabs but a litle ```killall NetworkManager``` ```sudo systemctl stop NetworkManager``` seemed to do the trick if they were fighting with each other.

Going into ```/etc/netctl``` to delete some of the old profiles and edit them seemed to be necessary, but now looking back, most of this was probably just flailing during the wifi setup.

There was one necessary setting however that wasn't fixed on startup for me. There were some errors launching ```sudo wifi-menu``` These were fixed with adding a config file to my wpa_supplicant. This is relatively easy to do by running ```wpa_supplicant -B -i wlp3s0 -d /path/to/config``` 

After running this command multiple times I ended up getting errors with competing processes and had to make sure to kill off old processes with ```killall wpa_supplicant```.

but there were a few issues connecting to the network of my school. I kept getting errors with connecting to websites. 

I tried to run ```Ping -c 3 www.google.com ``` after having issues at first but Ping would not return a DNS address for google and instead was returning a domain name that appeared to be associated with my school.

After coming almost to my wits end I ended up opening the redirect ip address in my browser, and found that it was a simple online registration form that for some reason did not work on linux so well.

After spending hours hunting down documentation and linux wifi forum questions I was schocked that my wifi "problems" were simple authentication issues like adding my device mac address to my organization internet account.

**Hardware goodies**
I ended up getting the high resolution display and this recquires you to get the IR cameras. While not something that ends up being terribly useful for me, you can access these cameras on  linux using ffmpeg videoforlinux2. The 340x340 mode will give you flashing frames with both IR sensors running and the 640x480 mode will give you a dark grainy IR camera feed from your cameras. You can also use guvcviewer or some other program to stream video from any of your webcams. There is also an _ok_ window hello replacement by the name of howdy that can be installed and works with T480 IR cameras.

Getting information from either of the batteries is also pretty easy too. Simply run ```upower -i /org/freedesktop/UPower/devices/battery_BAT{0,1}``` for the first battery or the second battery respectively.


---
[archlabs]: https://archlabslinux.com/ 
