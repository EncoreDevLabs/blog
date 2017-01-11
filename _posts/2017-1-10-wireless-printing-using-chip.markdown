---
layout: post
title:  "Wireless Printing using CHIP"
date:   2017-1-10 22:36:00
categories: chip printserver
image: /assets/article_images/2017-1-10/printing.jpg
image2: /assets/article_images/2017-1-10/printing-mobile.jpg
---

I had ordered [CHIP](https://getchip.com/pages/chip) sometime ago and wanted to share an interesting usecase for it to be used as a Print Server for your old printers. Using CHIP you can convert your ordinary printer without wireless capability into a wireless printer that can also take AirPrint commands from your iOS devices for as little as $9.

If you dont know what CHIP is then it is a [System on a chip](https://en.wikipedia.org/wiki/System_on_a_chip) similar to Raspberry Pi which has a decent processor, 512MB of RAM, 4GB built in storage for storing and booting the OS and a bluetooth and wireless card, all of which costs $9. Considering how expensive print servers are, this is a great device that can be made to act as a print server since it has all of the components we need. 

In order to get started, first you will need to install some service on it that can expose the printer connected to the CHIP to the network it is connected to. [CUPS](https://www.cups.org/) is a great open source project by Apple that can be installed on Debian linux OS which will help us convert our CHIP into a printer server. Once installed you will need to connect your CHIP to the printer and have your CHIP connected to your wifi network and then you should be able to see the option to print via AirPrint on your iOS device and also be able to add a wireless printer on your Mac or PC.

Hope this helps anyone looking for ideas for uses of their CHIP and for anyone who is trying to build a cheap Print Server at home.
