---
layout: page
title: "Automation for rainy days"
description: "Basic example how to use weather conditions to set states"
date: 2015-10-08 19:05
sidebar: true
comments: false
sharing: true
footer: true
ha_category: Automation Examples
---

### {% linkable_title Rainy Day Light %}

This requires a [forecast.io](components/sensor.forecast/) sensor with the condition `weather_precip` that tells if it's raining or not.

Turn on a light in the living room when it starts raining, someone is home, and it's afternoon or later.

```yaml
automation:
  alias: 'Rainy Day'

  trigger:
       - platform: state
         entity_id: sensor.weather_precip
         state: 'rain'
       - platform: state
         entity_id: group.all_devices
         state: 'home'
       - platform: time
         after: '14:00'
         before: '23:00'

  condition: use_trigger_values

  action:
    service: light.turn_on
    entity_id: light.couch_lamp
```

And then of course turn off the lamp when it stops raining but only if it's within an hour before sunset.

```yaml
automation 2:
  alias: 'Rain is over'
  trigger:
       - platform: state
         entity_id: sensor.weather_precip
         state: 'None'
       - platform: sun
         event: 'sunset'
         offset: '-01:00:00'

  condition: use_trigger_values
  action:
    service: light.turn_off
    entity_id: light.couch_lamp
```

