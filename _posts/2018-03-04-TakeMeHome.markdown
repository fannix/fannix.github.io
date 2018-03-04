---
layout: post
title:  "TakeMeHome Documentation"
date:   2018-03-04 21:37:31 +0000
categories: blockchain
---
TakeMeHome is an android app that helps to find missing kids or pets. It leverages the NEO blockchain technology and the beacon technology to create a search network. It shows how blockchain techonology can improve the general welfare of the socity. It is a show case of decentralized autonomous organization (DAO).

Trafficking of children is a serious problem, the International Labour Organization estimates that 1.2 million children are trafficked each year. 

The challenge is 1. to identify the missing subject 2. to locate the missing subject. 3. to mobilize as many as possible persons to make the join effort to bring back the missing subject.

An innovative use of beacon technology. Beacon technologies include iBeacon and other similar technologies such as Eddystones and altbeacons. Regardless of the brands, they are based on Bluetooth low energy proximity sensing by transmitting a universally unique identifier picked up by a compatible app or operating system. The identifier and several bytes sent with it can be used to determine the device's physical location, track customers, or trigger a location-based action on the device.

Comparing to other devices that provides tracking functionality such as GPS. Beacons are cheap, light consume very few power, can last for 1 or 2 years, have a acceptable range of signal coverage, usually up to several hundred metres.

Knowing the location of the missing subject is helpful. But ultimately, for the parent of missing child, or the owner of the lost cat, the most important thing is to bring it back safely, not to know where it is. To this end, we need an effective way to motivate people, especially the people closed to the current detected location, to search the kids or the pets.

NEO is a revolutionary way to create a decentralized autonomous organization (DAO). No trusted third party is necessary to organize the joint search. Once the child or pet is missing, the parent or the owner can set up and activate the bounty task, and then every TakeMeHome app will start searching the missing subject immediately. This is the best way to use the precious time to find the missing persons or pets, before it is too late and the missing subject have been brought to the next town or city. To reward the people who contribute to the searching, persons who are able to detect the beacons, thus locate the missing one will get some digital currency in return. And person who are help to bring the missing subject to home will get a large amount of digital currency. The reward mechanism is determined by the deployed smart contract.

Design and implementation 

The role of mainactivities and TakeMeHome is to initialize the app context and prepare for nearby beacons. Once detected, If the app is in the background, a notification is sent. 

If the app is in foreground or the notification is clicked. TrackingActivities will be used to track the movement of the beacon in realtime.

This app uses the excellent o3 android library for interacting with NEO blockchain. alt beacon library is used to detect and track beacons.

social benefit.

future improvement. 

NEO doesn't support making transaction inside the smart contract. 
Lack of development and debugging tools.
limited functionality inside smart contract.

moral hazard.

