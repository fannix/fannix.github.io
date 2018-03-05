---
layout: post
title:  "TakeMeHome: An App that Saves Children"
date:   2018-03-04 21:37:31 +0000
categories: blockchain
---
## Introduction

A missing kid (or pet) is the worst nightmare that could happen to a happy family. In the United States, an estimated 460,000 children are reported missing every year. And in the most unfortunate cases, many missing kids are actually abducted and trafficked, and the International Labour Organization estimates that 1.2 million children are trafficked each year. 

Usually, law enforcement and government takes the main responsibility of bringing the children back. In reality, they often need to work on multiple cases at the same time, and it limits their time that can be spent on any specific case. But we believe that to bring every missing kid home is of the utmost importance. And with the new technology, we are able to empower people and take some responsibility of finding missing kids. We also believes that we need to reward people that take
these responsibility in order to get more people involve in. 

To achieve such goals, several block issues need to be solved. These include:
1. how to identify the missing subject.
2. how to locate the missing subject. 
3. how to mobilize as many people as possible to join this effort.

TakeMeHome is an android app that helps to find missing kids or pets. It leverages the Beacon technology and the NEO blockchain technology cleverly to create a missing-kids-search network.
Beacons are used to identify missing subjects and determine their location. NEO blockchain is used to give the people who make contributions the well-deserved credit and make sure the network stay healthy and can grow.

This application shows that blockchain is not just a up-and-coming technology for economy life, but can also be used to improve the general welfare of the society. 




## Design and implementation 

### Architecture ###

![invoke](/images/diagram.png)

The application consist of 3 components.
1. Beacons that are carried by the children.
2. Smart Contracts that are deployed onto the NEO blockchain. 
3. An Android app that listens for broadcast message of beacons, and call the contracts when it receives eligible messages.

### Beacons ####

![invoke](/images/beacon.png)

Beacon technologies include iBeacon and other similar technologies such as Eddystones and altbeacons. Regardless of the brands, they are all based on Bluetooth low energy technology. Beacons can act as proximity sensors by transmitting a universally unique identifier, which can then be picked up by a compatible app or operating system. 
The identifier and several bytes sent with it can be used to determine the device's physical location, track customers, or trigger a location-based action on the device.

Comparing to other devices that provides tracking functionality such as GPS, beacons have the following advantges:
1. small and light. It can be easily the carried by an infant or a small dog. As shown on the above image, the blue beacon is about the same size as a coin.
2. cheap and affordable by almost every family. It normally costs less than 20 dollars. 
3. it consumes very little power and one new battery can last for more than 1 year. This keeps the window of saving the kids open for a very long time.  
4. it has a acceptable range of signal coverage, usually up to several hundred meters.


### Smart Contract ###

Knowing the location of the missing subject is helpful. But ultimately, for the parent of missing child, or the owner of the lost cat, the most important thing is to bring it back safely, not to know where it is. To this end, we need an effective way to motivate people, especially the people closed to the current detected location, to search for the kids or the pets.

NEO is a revolutionary way to enable a decentralized autonomous organization (DAO) without trusted third parties involved. Once the child or pet is missing, the parent or the owner can set up and activate the bounty task,
and then every TakeMeHome app will start searching the missing subject immediately. This is the best way to use the very precious time to find the missing persons or pets, before it is too late and the missing subject have been brought to the next town or city. 
To reward the people who contribute to the searching, persons who are able to detect the beacons, 
thus locate the missing one will get some digital currency in reward. And person who are help to bring the missing subject to home will get a large amount of digital currency. The reward mechanism is determined by the deployed smart contract.

We deploy two smart contracts.
1. The first one is a registry for missing children. This is basically a key value table. The key is the hash of the beacon ID. The reason we use the hash instead of the plain text of beacon ID is to make sure the people who claims are in the proximity region of the children are not reporting incorrect current location by using fake beacon ID.
The value is the address of the smart contract that encodes the reward rules masked by a modified beacon ID string. This is also to ensure the people who calls the reward contract indeed are in the proximity region.
2. The second one  has several purposes. 
    - It stores the URL of web page which has the photo and description of the missing subject.
    - It stores the email of the contact person.
    - It stores the historical reported location of the children or the pets.
    - It describes how people submitting the location will be rewarded.

You can find codes for both contracts here: <https://github.com/fannix/TakeMeHomeContract>

### Android App ###


The app consist of 4 Java files and a few libraries.


The responsibility of `MainActivity` and `TakeMeHome` are to initialize the app context and detect beacons in background.

![invoke](/images/mainActivity.png)

Once detected, If the app is in the background, a notification will be sent to the running device. If the app is in foreground or the notification is clicked. `TrackingActivity` will be brought up onto the foreground. 
This activity will conduct the following steps:
1. Query the registry contract and determine whether the detected beacon is registered as missing.
2. Use the beacon ID (shown in the image below) to retrieve the masked reward contract address and unmask it.
3. Get the URL and the email (doge@doge.com for this beacon) from the reward contract. 
4. Displays information about the missing subject.
5. Submit the current location
6. Show the real-time distance between the device and the beacon and track the movement of the beacon.
This can help the device owner make a more informed decision, like whether to call police immediately, or continue to follow the children.

![invoke](/images/trackingActivity.png)

Clicking the Send Email button will bring up the email program. You can add extra information about the location or circumstance here to send to the contact person. 

![invoke](/images/email.png)

`API` is the class that talks with the NEO blockchain. A large part of the communication is delegated to the excellent `O3Android` library for interacting with NEO blockchain. `AltBeaconLibrary` is used to detect and track beacons.

The codes for the app are also in github: <https://github.com/fannix/TakeMeHome>


## Future Improvement. 

We identify a few potential improvements for this application.

1. NEO doesn't support transferring native assets inside the smart contract yet. This force us to settle for a less obvious solution to distribute rewards. Basically we are creating IOUs, instead of sending assets directly. 
We are finding a better way to bypass the limitation.
2. Calling smart contracts with RPC takes a few seconds to get response. Hence the app is not very responsive. We need to optimize this aspect.


In the process of development, there are a few limitation from NEO sides that we feel can be improved:

1. Lack of mature development and debugging tools.
2. The documentation is still limited.
