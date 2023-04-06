This repository contains a Battery component for [ESPHome](https://esphome.io/)
to support the ESP32-S3 [LILYGO T5 4.7" Plus E-paper display](https://www.lilygo.cc/products/t5-4-7-inch-e-paper-v2-3).

(Do not confuse it with the original ESP32-based Lilygo T5 4.7 board.)

## Usage

To use the board with [ESPHome](https://esphome.io/) **you have to put quite a
number of options in your esphome config**:
* Configure the aprpopriate board, variant, and framework versions in the
[esp32 platform](https://esphome.io/components/esp32.html)
* Set a bunch of `platformio_options`
* Include the component from this repository as `external_components` 

```yaml
# ... required esp32, platformio_options configuration omitted for brevity ...

external_components:
  - source: github://kaeltis/esphome-lilygo-t547plus
    components: ["lilygo_t5_47_battery"]

sensor:
  - platform: lilygo_t5_47_battery
    id: battery_voltage
    voltage:
      name: "Battery Voltage"

  - platform: template
    name: "Battery Percentage"
    id: battery_percentage
    unit_of_measurement: "%"
    accuracy_decimals: 0
    device_class: battery
    lambda: |-
      int y = (1-(4.1-id(battery_voltage).voltage->state)/(4.1-3.3))*100;
      if (y < 100) {return y;} else {return 100;};
```

## Discussion

https://github.com/esphome/feature-requests/issues/1960
