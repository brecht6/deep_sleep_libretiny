## deep_sleep_libretiny
This is a copy of the esphome's deep_sleep component implementation for libretiny. It was taken from esphome-libretiny (an early fork of esphome), where it was posted as a PR that never made it to esphome.
The code has been cleaned-out to leave only what relates to the beken bk7231 chips.

## Usage
You use this component similarly as you would use the usual esphome's deep_sleep, with a few modifications:
1) Specify `deep_sleep_libretiny` instead of `deep_sleep`
2) As you can use many wake-up pins on beken chips, you specify them via `wakeup_pins` (instead of `wakeup_pin`).

```yaml
deep_sleep_libretiny:
  id: deep_sleep_lt
  run_duration: 30s
  sleep_duration: 24h
  wakeup_pins:
    - pin:
        number: P20
        allow_other_uses: true
        mode: 
          input: True
          pullup: True
        inverted: True
      wakeup_pin_mode: KEEP_AWAKE
    - pin:
        number: P16
        allow_other_uses: true
        mode: 
          input: True
      wakeup_pin_mode: INVERT_WAKEUP

...
ota:
  - platform: esphome
    on_begin:
      then:
        - deep_sleep_libretiny.prevent: deep_sleep_lt
```

# issues
When the original deep_sleep component is used, esphome seamlessly enables a flag that tells Home Assistant the devices uses deep sleep. This enables HA mechanism that prevents showing the sleeping device as unavailable.
I have found no way to enable this flag from external component, therefore the above mechanism does not work and in result the device shows up as unavailable.
