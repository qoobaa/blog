---
layout: post
title: USB Type C in Practice
hacker_news_id: 0
---

I've recently replaced a Nexus 5X with a Xiaomi Mi 5, just before 5X
dies in a bootloop like everyone else's does. Both devices have USB
Type C ports, so you can just use whatever Type C charger for both
without worrying which one is for LG and which one is for Xiaomi,
right?  Well, almost…

When you connect the Xiaomi's charger (black cable) to the Nexus
phone, and Nexus' charger (white cable) to Xiaomi, both phones charge
properly - you can see "Charging" in the bottom of both screens.

![Charging](/i/MVIMG_20180615_064341.jpg)

They charge properly except one tiny little detail. Both devices have
"rapid charging" feature, which promises to charge the battery much
quicker than normally. Let's swap the cables.

![Charging Rapidly](/i/MVIMG_20180615_064402.jpg)

Let's have a look at the chargers then.

![Chargers](/i/MVIMG_20180615_064810.jpg)

The LG "Travel Adapter" on the left, is capable of delivering 3 A of
current at 5 V, but the Xiaomi's one turns out to be a QuickCharge 3.0
charger instead. Xiaomi "Adaptor" on the right provides variable
output of 5 V/2 A, 9 V/1.2 A or 12 V/1 A.

It gets even more interesting when you connect the Xiaomi to a USB PD
enabled charger - it **won't charge at all** because Mi 5 doesn't
support PD.

*[USB]: Universal Serial Bus
*[PD]: Power Delivery
