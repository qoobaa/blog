---
layout: post
title: G13 Tariff in Home Assistant
hacker_news_id: 33382263
---

I've heard a couple of stories recently about people switching to
[G13](https://www.tauron.pl/-/media/offer-documents/produkty/prad-z-serwisantem-dokumenty/ts/wyciag-typu-g-z-taryfy-td-sa-na-rok-2021.ashx)
energy tariff saving money on energy bills even without changing their
energy usage patterns and habits. I've decided to check it on my own,
as I already have an energy meter connected to Home Assistant. As the
first step, I thought it'd be a good idea to split the counter into
three categories: morning peak, afternoon peak, and off-peak. It'd
allow me to compare it to the regular G11 tariff at the end of the
month.

{% raw %}
~~~ yaml
sensor:
  - platform: template
    sensors:
    energy_tariff_name:
      friendly_name: G13 Tariff Name
      value_template: >
        {% set month = now().strftime('%m') | int %}
        {% set hour = now().strftime('%H') | int %}
        {% set isSummer = month >= 4 and month <= 9 | bool %}
        {% set isWinter = month <= 3 or month >= 10 | bool %}
        {% set isWorkday = states('binary_sensor.workday_sensor') | bool %}
        {% if isWorkday and hour >= 7 and hour < 13 %}
          morning_peak
        {% elsif isWorkday and (isSummer and hour >= 19 and hour < 22 or isWinter and hour >= 16 and hour < 21) %}
          afternoon_peak
        {% else %}
          off_peak
        {% endif %}
~~~
{% endraw %}

The hardest part of figuring out the tariff name is the workday
part - G13 assumes all weekends and public holidays as off-peak
hours. Fortunately, Home Assistant comes to the rescue with an
easy-to-use workday sensor, defined as follows:

~~~ yaml
binary_sensor:
  - platform: workday
    country: PL
~~~

We'd need to list the tariffs in the utility meter section:

~~~ yaml
utility_meter:
  daily_energy:
    source: sensor.home_total_daily_energy
    cycle: daily
    tariffs:
      - morning_peak
      - afternoon_peak
      - off_peak
  monthly_energy:
    source: sensor.home_total_daily_energy
    cycle: monthly
    tariffs:
      - morning_peak
      - afternoon_peak
      - off_peak
~~~

The last part is the actual tariff selection. The automation runs
every hour, getting the tariff name from the template sensor defined
above, assigning it to the `daily_energy` and `monthly_energy`
entities.

{% raw %}
~~~ yaml
- id: '1666861689467'
  alias: Set G13 Tariff
  description: ''
  trigger:
    - platform: time_pattern
      minutes: '0'
  condition: []
  action:
    - service: select.select_option
      data:
        option: "{{ states('sensor.energy_tariff_name') }}"
      target:
        entity_id: select.daily_energy
    - service: select.select_option
      data:
        option: "{{ states('sensor.energy_tariff_name') }}"
      target:
        entity_id: select.monthly_energy
  mode: single
~~~
{% endraw %}

![G13 Tariff Name in Home Assistant](/i/g13_tariff_name.jpg)

I'll try to write a follow-up post in a month or so with a comparison
of G11 and G13 tariffs.
