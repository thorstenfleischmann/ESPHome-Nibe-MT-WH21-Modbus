# ESPHome/Home Assistant Nibe-MT-WH21 Modbus configuration

Nibe MT-WH21 Domestic hot water heat pump ESPHome modbus configuration for control with Home Assistant or ESPhome web interface.

Adjust to your needs. Use at own risk.

```
esphome:
  name: Brauchwasserwaermepumpe

esp32:
  board: nodemcu-32s
  framework:
    type: arduino

...

uart:
   id: uart_1
   tx_pin: GPIO17 
   rx_pin: GPIO16 
   baud_rate: 19200
   stop_bits: 1
   parity: EVEN

modbus:
   id: modbus_1
   uart_id: uart_1
   send_wait_time: 1000ms

modbus_controller:
 - id: bwwp
   address: 30
   modbus_id: modbus_1
   setup_priority: -10
   update_interval: 60s

select:
  - platform: modbus_controller
    name: "Mode (Auto, Eco)"
    address: 0
    #register_type: "holding"
    value_type: U_WORD
    optionsmap:
      "Auto": 0
      "Öko": 1
      "Boost": 2
      "Backup": 3
      "Silent": 4
      "Ferien": 5

number:
  - platform: modbus_controller
    name: "Set T Auto"
    address: 8
    register_type: "holding"
    unit_of_measurement: "°C"
    value_type: S_WORD
    multiply: 10
    min_value: 45
    max_value: 60

  - platform: modbus_controller
    name: "Set T Eco"
    address: 9
    register_type: "holding"
    unit_of_measurement: "°C"
    value_type: S_WORD
    multiply: 10
    min_value: 45
    max_value: 55

  - platform: modbus_controller
    name: "Set T Boost"
    address: 10
    register_type: "holding"
    unit_of_measurement: "°C"
    value_type: S_WORD
    multiply: 10
    min_value: 50
    max_value: 65


sensor:
  - platform: modbus_controller
    name: "T air in"
    address: 11
    register_type: "read"
    unit_of_measurement: "°C"
    value_type: S_WORD
    accuracy_decimals: 1
    filters:
    - multiply: 0.1

  - platform: modbus_controller
    name: "T air out"
    address: 12
    register_type: "read"
    unit_of_measurement: "°C"
    value_type: S_WORD
    accuracy_decimals: 1
    filters:
    - multiply: 0.1

  - platform: modbus_controller
    id: "watertop"
    name: "T water top"
    address: 13
    register_type: "read"
    unit_of_measurement: "°C"
    value_type: S_WORD
    accuracy_decimals: 1
    filters:
    - multiply: 0.1

  - platform: modbus_controller
    name: "T water low"
    address: 14
    register_type: "read"
    unit_of_measurement: "°C"
    value_type: S_WORD
    accuracy_decimals: 1
    filters:
    - multiply: 0.1

  - platform: modbus_controller
    name: "SG2 Input"
    address: 26
    register_type: "read"
    value_type: U_WORD

  - platform: modbus_controller
    name: "Status compressor"
    address: 22
    register_type: "read"
    value_type: U_WORD

  - platform: modbus_controller
    name: "Status el heater"
    address: 23
    register_type: "read"
    value_type: U_WORD

  - platform: modbus_controller
    name: "El Power"
    address: 30
    register_type: "read"
    unit_of_measurement: "W"
    value_type: S_WORD

  - platform: modbus_controller
    name: "Total Power"
    address: 31
    register_type: "read"
    unit_of_measurement: "W"
    value_type: S_WORD


```
