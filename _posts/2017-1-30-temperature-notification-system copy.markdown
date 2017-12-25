---
layout: post
title:  "Indoor Temperature Notification Project"
date:   2017-1-30 2:39:00
categories: feature sustainablejersey raspberrypi home-automation
image: /assets/article_images/2017-1-30/sustainable.jpg
image2: /assets/article_images/2017-1-30/sustainable-mobile.jpg
---

I came across a non profit organization called [Sustainable Jersey](http://www.sustainablejersey.com/) and they are an organization that provide tools, support and incentives to support communities that pursue sustainable programs. Recently in partnership with AT&T they created a competition to kick start the sustainable movement. They partnered with several townships in NJ to ask them for their problems and brought them in front of technologist like me or you who are interested in helping their or other community out by solving their problem with technology.

I decided to attend the [kick off event](http://cfc.sustainablejersey.com/) hosted at NJIT on Jan 27th and met great people who work in the government or public organizations who need help solving their problem using technology but do not have the resources or budget to do so. One such problem that interested me was an overheating problem of rooms at a public school in Maplewood NJ. The public school had a need to notify in real time the building administrative staff so that they can have the problem be fixed right away as currently it took them days to notice and get it fixed. I decided to pursue and solve this problem by building a prototype first at home where the system I want to build can notify me when the temperature of a certain room where I have the sensor goes above or below a certain limit and also to notify me via text message or phone call as a real time notification system.

To solve this problem I decided to use Raspberry Pi as it is a very versatile device that can connect to a Wifi network and you can also attach a temperature sensor to it using the GPIO and send that data over the internet to a server which manages the whole system of keeping an inventory of all of the sensors, which room they belong to, which location those rooms are in, which organization those locations belong to and who are the users responsible in those organization or locations along with their contact info to notify them.

I will go through a series of blog post describing how I build this system for my own home and how you can try to build something similar if you are interested. Also if anyone is in NJ and want to contribute to the community then they can also participate in the [Coding for Community competition](http://cfc.sustainablejersey.com/) hosted by Sustainable Jersey as it is not to late to get started.
