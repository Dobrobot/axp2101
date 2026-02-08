# ESPHome AXP2101 component

Custom ESPHome component for the X-Powers AXP2101 PMU. It is used on M5Stack Core2 v1.1
and exposes battery voltage/level/charging state plus backlight control (BLDO1) via
the `brightness` setting.

## Backlight control (BLDO1)

Core2 v1.1 uses AXP2101 BLDO1 to power the LCD backlight (LED_A). This component maps
`brightness` (0.0..1.0) to BLDO1 output voltage (0.5..3.5V). Zero disables BLDO1.
This makes it possible to control backlight brightness via the PMU, which is not
available through the display controller registers.

## Example usage

```yaml
external_components:
  - source:
      type: git
      url: https://github.com/Dobrobot/axp2101.git
      ref: main
    components: [axp2101]

i2c:
  - id: bus_internal
    sda: GPIO21
    scl: GPIO22
    scan: true
    frequency: 400kHz

sensor:
  - platform: axp2101
    id: axp2101_pmu
    model: M5CORE2
    address: 0x34
    i2c_id: bus_internal
    update_interval: 30s
    brightness: 75%
    battery_voltage:
      name: "Battery Voltage"
    battery_level:
      name: "Battery Level"
    battery_charging:
      name: "Battery Charging"
```
