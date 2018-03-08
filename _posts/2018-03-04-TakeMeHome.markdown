---
layout: post
title:  "TakeMeHome: An App that Saves Children"
date:   2018-03-04 21:37:31 +0000
categories: blockchain
---
## Introduction

There is nothing more devastating to a happy family than the parents find their beloved child went missing. However, hundreds of families go through this each day. In the United States alone, an estimated 460,000 children are reported missing every year ([source](http://globalmissingkids.org/awareness/missing-children-statistics/)). And in the most unfortunate cases, many missing kids are actually abducted and trafficked, and the International Labour Organization estimates that 1.2 million children are trafficked each year ([source](https://www.unicef.org/protection/57929_58005.html)).

Usually, law enforcement system and government take the main responsibility of bringing these children back. In reality, they often need to work on multiple cases at the same time,
and their time that can be spent on any specific case is very limited.
But to bring each missing kid home is of the utmost importance to his or her family.
And we believe that with technology innovation, people should be empowered and everyone can take part in such effort. 

TakeMeHome is an android app that anyone can use to find missing kids or pets. It leverages the Beacon technology and the NEO blockchain technology cleverly to create a missing-kids-search community.
Beacons are used to identify missing subjects and determine their location. NEO blockchain is used to give the people who make contributions the well-deserved credit and help the community to stay healthy and grow.

This application shows that blockchain technology not only plays an important role in economy, but also helps to improve the general welfare of the entire world. 

## Challenges 

Building such an app is by no means an easy task. Several blocking issues need to be solved. These include:
1. how to identify the missing subject.
2. how to locate the missing subject. 
3. how to mobilize as many people as possible to join this effort.

In the following section, we will show how we tackle these issues via an innovative combination of Beacon technology and blockchain.


## Design and implementation 

### Architecture

![invoke](/images/diagram.png)

The application consists of 3 components.
1. Beacons that are carried by the children.
2. Smart Contracts that are deployed onto the NEO blockchain. 
3. An Android app that listens for broadcast message of beacons, and call the contracts when it receives eligible messages.

### Beacons

![invoke](/images/beacon.png)

Beacon technologies include iBeacon and other similar technologies such as Eddystones and altbeacons. Regardless of the brands, they are all based on Bluetooth low energy technology. Beacons can act as proximity sensors by transmitting a universally unique identifier, which can then be picked up by a compatible app or operating system. 
The identifier and several bytes sent with it can be used to determine the device's physical location, track customers, or trigger a location-based action on the device.

Comparing to other devices that also provide tracking functionality such as GPS, beacons have the following advantages:
1. small and light. As shown on the above image, a beacon can be as small as a coin. Therefore they can be easily carried by an infant or a small dog. 
2. cheap and affordable by almost every family. It normally costs less than 20 dollars. 
3. it consumes very little power and one new battery can last for more than 1 year. This keeps the window of saving the kids open for a very long time.  
4. it has an acceptable range of signal coverage, usually up to several hundred meters. Hence there will always be somebody nearby can pick up the signal.


### Smart Contract

Knowing the location of the missing subject is helpful. But ultimately, for the parent of missing child, or the owner of the lost cat, the most important thing is to bring it back safely. To this end, we need an effective way to motivate people, especially the people closed to the current detected location, to search for the kids or the pets.

NEO is a revolutionary way to create a decentralized autonomous organization (DAO) without trusted third parties involved. If the child or pet was missing, the parent or the owner can set up and activate the bounty task via a smart contract,
and then every TakeMeHome app will start searching the missing subject immediately. This is the best way to utilize the very precious time, before it is too late and the missing subject has been taken to other towns or cities. 

To reward the people who contribute to the searching, smart contracts are also used to reward persons who are able to detect the beacons and provide valuable information about where the missing ones are. The reward mechanism is hard-coded in the deployed smart contract, thus avoid any potential ambiguity and dispute.

We deploy two smart contracts.
1. The first one is a registry for missing children. This is basically a key value table. The key is the hash of the beacon ID. The reason we use the hash instead of the plain text of beacon ID is to make sure no one can report incorrect location by counterfeiting the beacon.
The value is the address of the smart contract that encodes the reward rules masked by a modified beacon ID string. This is also to ensure the people who calls the reward contract indeed are in the proximity region of the beacon.
2. The second one  has several purposes. 
    - It stores the URL of web page which has the photo and description of the missing subject.
    - It stores the email of the contact person.
    - It stores the historical reported location of the children or the pets.
    - It describes how people submitting the location will be rewarded.

You can find codes for both contracts here: <https://github.com/fannix/TakeMeHomeContract>

### Android App


The app consist of 4 Java files and a few libraries.


The functionalities of `MainActivity` and `TakeMeHome` are to initialize the app context and search for beacons in background.

![invoke](/images/mainActivity.png)

If a beacon is detected , a notification will be sent to the running device and `TrackingActivity` will be brought up to the foreground. 
This activity will conduct the following steps:
1. Query the registry contract and determine whether the detected beacon is registered as missing.
2. Use the beacon ID (shown in the image below) to retrieve the masked reward contract address and unmask it.
3. Get the URL and the email (doge@doge.com for this beacon) from the reward contract. 
4. Displays information about the missing subject.
5. Submit the current location.
6. Show the real-time distance between the device and the beacon and track the movement of the beacon.
This can help the device owner to make a more informed decision, he or she could call the police immediately, come forward to help the child, or follow the child.

![invoke](/images/trackingActivity.png)

Clicking the Send Email button will bring up the email program. You can add extra information about the location or circumstance here to send to the contact person. 

![invoke](/images/email.png)

`API` is the class that communicates with the NEO blockchain. A large part of the communication is delegated to the excellent `O3Android` library for interacting with NEO blockchain. `AltBeaconLibrary` is used to detect and track beacons.

The codes for the app are also in github: <https://github.com/fannix/TakeMeHome>


## Contribution

1. Targets and solves a very important social problem
2. A good example of how NEO can be used to benefit general public
3. An innovative combination of technologies: blockchain + beacon + mobile


## Future Work 

We identify a few potential improvements for this application.

1. NEO doesn't support transferring native assets inside the smart contract yet. This force us to settle for a less obvious solution to distribute rewards. Basically we are creating IOUs, instead of sending assets directly. 
We are finding a better way to bypass the limitation. One approach we might consider is to mint tokens for this application to distribute rewards.
2. Calling smart contracts with RPC takes a few seconds to get response. Hence the app is not very responsive. We need to optimize this aspect.


In the process of development, there are a few limitation from NEO sides that we feel can be improved:

1. Lack of mature development and debugging tools.
2. The documentation is still limited.
