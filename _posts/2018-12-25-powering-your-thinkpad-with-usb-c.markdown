---
layout: post
title: Powering Your ThinkPad with USB C
hacker_news_id: 18756776
---

I currently work on a [ThinkPad T450](https://thinkwiki.de/T450),
after switching back from Dell XPS series recently. It's a decent
piece of hardware in general, it has everything I could expect from an
ultrabook except for one tiny little detail. For me, the main reason
to upgrade to a newer model would be the USB C port so I could have a
single power adapter for all my devices. Fortunately, there's an easy
way to fix it.

![Adapter](/i/IMG_20181018_101721.jpg)

I've made a USB C to Dell converter using [PD Buddy
Sink](https://www.tindie.com/products/clayghobbs/pd-buddy-sink/)
before, and it worked great. The board costs around $45 including
shipping to EU, and I would highly recommend it. However, I've found a
smaller and cheaper alternative to PD Buddy Sink from China. The
converter above is made using
[ZY12PDS](https://www.aliexpress.com/item/ZY12PDS-Type-C-PD-to-DC-USB-SurfacePro-decoy-fast-charge-trigger-polling-device/32914462770.html),
you can get it for less than $15 including shipping worldwide. It's a
bit less flexible than the PD Buddy Sink since it allows you to choose
only 9, 12, 15 or 20 V (20 V is the default setting), but it's just
good enough to power most of your devices.

**Warning notice: please double check the polarity after soldering the
Lenovo plug to avoid damaging your laptop's mainboard.**

*[USB]: Universal Serial Bus
*[PD]: Power Delivery
