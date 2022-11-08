---
layout: post
title: Energy Consumption Measurement in Home Assistant
hacker_news_id:
---

Referring to the
[post](https://blog.kubakuzma.com/2022/10/29/g13-tariff-home-assistant.html)
I published a couple of weeks ago, I've decided to share my way of
measuring energy consumption. I've got a pretty standard setup
consisting of a 3-phase energy meter outside the flat. It's not very
handy when it comes to integrating it with Home Assistant, so I bought
[a cheap 3-phase energy
meter](https://orno.pl/en/product/1083/3-phase-energy-meter-with-mid-80a-3-modules-din-th-35mm)
instead and installed it internally. It has a single pulse output with
800 pulses per kWh, and it costs less than $40 (1-phase version is
available too). The pulse output is quite an important feature -
having it exposed is much more convenient than interfacing with a
blinking LED.

The cheapest way of counting pulses I've found is adding a simple
[Sonoff Mini
R2](https://sonoff.tech/product/diy-smart-switches/minir2/) for less
than $10, and using its "switch terminals" for pulse detection. It
works flawlessly and it's easy to set up in
[ESPHome](https://esphome.io/components/sensor/pulse_counter.html):

~~~ yaml
sensor:
  - platform: pulse_counter
    id: home_power
    pin: GPIO4
    unit_of_measurement: "kW"
    name: "Home Power"
        filters:
      - multiply: 0.075 # 60/800
    total:
      accuracy_decimals: 5
      unit_of_measurement: "kWh"
      name: 'Home Total Energy'
      filters:
        - multiply: 0.00125 # 1/800
~~~

This setup costs less than $50 in total and it's actually quite
reliable - it's composed of off-the-shelf components only, no need for
soldering.
