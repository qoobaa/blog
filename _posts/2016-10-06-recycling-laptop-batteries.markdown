---
layout: post
title:  "Recycling Laptop Batteries"
hacker_news_id: "?"
---

I've been recycling laptop batteries for a while now, and I thought it'd be a good idea to summarise things I've learned so far.

**1. The initial voltage of a cell doesn't always determine its shape.**

Before I started the process, I found an information here and there, saying that Li-Ion structure gets irreversibly damaged if discharged below 3&nbsp;V. Some people suggest that cells below 2&nbsp;V should be automatically discarded and better not charged at all. What I can confirm for sure, is that if a cell is above 3&nbsp;V, it's more likely to be in a good shape. However, it isn't unusual to find healthy cells discharged to almost 0&nbsp;V. They require a little bit more work because your charger may not recognise them - especially when it's a NiMH/Li-Ion charger like mine (such cells are usually treated as NiMH-s). The easiest way to bring such a cell back to life is to balance it with another cell above 3&nbsp;V - just connect these cells in parallel for a minute, and insert to the charger after that. Most of them start charging normally, some of them are 2000+&nbsp;mAh.

**2. Check if a cell is able to maintain the voltage.**

Inability to maintain the voltage is usually a very bad sign. A healthy cell should be able to store the energy for years. I've seen many high capacity cells that dropped below 3.7&nbsp;V a week after being charged to 4.2&nbsp;V. It means that some of their energy got lost, and was slowly dissipated in heat. If you connect such a cell in parallel with healthy ones, it'll most likely be draining the rest of cells, warming itself up.

![Three Liitokala Lii-500 chargers in action](/i/IMG_20160915_122120.jpg)

**3. Internal resistance isn't very reliable information.**

I'm testing my batteries using [Liitokala Lii-500](http://bit.ly/2dO1poO) chargers, and I'm quite happy with them. Apart from the voltage, measured capacity and charging time, they also show cell's internal resistance in milliohms. I thought it might be useful information, but I quickly noticed that the resistance reads tend to be significantly different when measured multiple times for the same cell. The very same cell may be 120&nbsp;mΩ when you insert it in the charger for the first time, and drop to 60&nbsp;mΩ when you insert them after being fully charged. I believe my chargers are not very accurate in this matter, and measuring internal resistance may require a little bit more expensive tools. I've decided to completely ignore this value for now.

**4. Don't push your cells too hard.**

At the beginning, I used to charge my cells at 1&nbsp;A current to make the whole process a little bit quicker. It turned out to be a bad idea because some cells weren't able to survive the charging process. I figured out that some of them were really hot when they were getting close to 4.2&nbsp;V.    Things got way better when I switched to charging at 500&nbsp;mA current - no more dying cells and overheating problems. Now I'm also able to run 3 chargers connected to a single 12&nbsp;V, 3&nbsp;A power supply with no issues at all.

*[NiMH]: Nickel–metal hydride battery
