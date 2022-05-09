#Sensor, Command, Automation and Script Examples

Below is a list of examples, provided by the community.

---

#### Template trigger: LastActive

```yaml
alias: example
trigger:
- platform: template
  value_template: "{{ (now() - states('sensor.testvm_lastactive')|as_datetime).total_seconds() < 60 }}"
action:
   ... etc ...
```

Alternative version:

```yaml
  value_template: "{{ now() - states('sensor.testvm_lastactive')|as_datetime < timedelta(minutes=1) }}"
```

#### Condition: TTS only when microphone's not active

```yaml
tts_test:
  alias: Test TTS
  mode: single
  sequence:
    - condition: state
      entity_id: binary_sensor.test_vm_mic_active
      state: "off"
    - service: notify.mycroft_tts
      data_template:
        message: >-
          {{ message }}
```

#### Switch: Wake-On-LAN

```yaml
- platform: wake_on_lan
  name: "your_pc_name"
  mac: "00-00-00-00-00-00"
  host: 10.0.0.5
  broadcast_address: 10.0.0.255
  turn_off:
    service: switch.turn_on
    data:
      entity_id: switch.your_pc_hass_agent_shutdown_switch
```

Alternative version: use a `Hibernate Command` to put your PC in hibernation instead.

#### Sensor: GeoLocation

```yaml
alias: GPS voiture
description: ''
trigger:
  - platform: state
    entity_id: sensor.voiture_geolocation
condition: []
action:
  - service: device_tracker.see
    data:
      dev_id: Voiture
      gps:
        - '{{ states(''sensor.voiture_geolocation'').split(",")[0] }}'
        - '{{ states(''sensor.voiture_geolocation'').split(",")[1] }}'
mode: single
```

This places the GeoLocation sensor as a `device_tracker` on the map (thanks **Neilge**).