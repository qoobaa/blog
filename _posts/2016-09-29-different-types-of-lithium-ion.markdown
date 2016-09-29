---
layout: post
title:  "Different Types of Lithium-Ion"
hacker_news_id: 12604405
---

I didn't expect such a huge reaction to the [article I published recently](https://blog.kubakuzma.com/2016/09/22/how-does-a-kilowatt-hour-look-like.html). The article stayed on the Hacker News front page for more than 15 hours. In that time around 50k users have visited my blog. Thank you all for comments and suggestions - it really motivates me to write something regularly and to work on projects like this one.

It's no surprise for me that a lot of people mentioned how dangerous and pointless this project could be. Especially when it comes to building battery modules from lithium-ion cells of unknown origin, different chemistry, and so on. I agree that it might be dangerous - especially when you don't know what you're doing. On the other hand, building a power wall from recycled 18650 cells is not my idea - there's a bunch of people out there, [powering their homes using these batteries for some time](https://www.youtube.com/user/nocrf50here), and they seem to be still alive, their houses haven't exploded yet.

As it was mentioned in the comments, lithium-ion is a broad term - cells may have different chemistry which means different operating voltages, different charge and discharge characteristics. Take a look at the diagram below - the green bars show operating voltage ranges of different cell chemistries. Red bars are considered as overcharged or over-discharged cell voltages. Yellow bar in Lithium Cobalt Oxide is considered as over-discharged voltage in some cells - it's generally safer to cut them off when they're around 3&nbsp;V.

![Operating voltages of different Li-ion chemistries](/i/li-ion-chemistry.svg)

What you can see from the diagram is that most of the cells are safe to operate between 3&nbsp;V - 4.2&nbsp;V range. It's crucial before you start connecting multiple cells in parallel to make higher capacity modules. While it's generally possible to find an e.g. Lithium Iron Phosphate 18650 cell, I've never found a single one in around 200 cells from old laptop batteries I've recycled so far. The majority of the recycled cells are easily identifiable by googling their labels.

![Different cells recycled from laptop batteries](/i/IMG_20160928_104818.jpg)

In fact, most of them are ICR or lithium cobalt oxide cells (apart of the third one from the bottom, which is INR or lithium manganese nickel - it's a rare chemistry in laptop batteries). The lowest charge/discharge rate you can find browsing their specifications is around 0.7&nbsp;C. Assuming that most of them are over 2000&nbsp;mAh, it should be safe to charge/discharge them at 1.5&nbsp;A current (and actually most of them go beyond 2-3&nbsp;A). Considering that we'd like to connect a hundred or more cells in parallel, we'd need to draw 150&nbsp;A or more to go beyond these limits. It's rarely the case when you deal with standard 48&nbsp;V battery systems. Drawing 100&nbsp;A from a 48&nbsp;V power wall gives you roughly 5&nbsp;kW of power - that's enough power for most of your home appliances to run (at least when you run them wisely).
