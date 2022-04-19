# homebridge-modbus

Hombridge plugin for controlling appliances through ModbusTCP

You must define each accessory in the config.json file, with a `name`, a `type` (that matches any HomeKit Service type), and one or more characteristics (that match HomeKit Characteristics for this Service).
If you want to group several services under the same accessory, just reuse the same accessory name, and add a `subtype` field if services are of the same type.
The characteristics value in the json is the modbus address associated with it.
Coils are defined with the letter 'c' followed by the coil number, holding registers are defined with the letter 'r' followed by the register number, input registers are defined with the letter 'i' followed by the register number.

If you want more advanced control, the characteristic value can instead be an object having `address` as the coil/register type and number, and optional elements like `validValues`, `maxValue`, `value` to configure HomeKit, `mask`, `map`, `len`, `scale` to map different values between modbus and homekit, `readOnly` to force holding registers or coils to not be written on modbus (input registers are always read-only).

Example config.json:
```json
{
    "platform": "Modbus",
    "ip": "192.168.1.20",
    "port": 502,
    "pollFrequency": 5000,
    "modbus_mode": 1,
    "accessories": [
        {
            "name": "Außen",
            "type": "TemperatureSensor",
            "CurrentTemperature": {
                "address": "i10201",
                "scale": 100
            }
        },
        {
            "name": "Warmwasser",
            "type": "Thermostat",
            "TargetTemperature": {
                "address": "r13190",
                "scale": 10,
                "value": 55,
                "maxValue": 60.5
            },
            "CurrentTemperature": {
                "address": "i10206",
                "scale": 100
            },
            "CurrentHeatingCoolingState": {
                "address": "i4007",
                "map": {
                    "0": 0,
                    "1": 1
                },
                "readonly": true
            }
        },
        {
            "name": "Fussboden",
            "type": "Thermostat",
            "TargetTemperature": {
                "address": "r12180",
                "scale": 10,
                "value": 21,
                "maxValue": 23
            },
            "CurrentTemperature": {
                "address": "i10204",
                "scale": 100
            },
            "CurrentHeatingCoolingState": {
                "address": "i4010",
                "map": {
                    "0": 0,
                    "1": 1
                },
                "readonly": true
            }
        },
        {
            "name": "Radiatoren",
            "type": "Thermostat",
            "TargetTemperature": {
                "address": "r11180",
                "scale": 10,
                "value": 22,
                "maxValue": 23
            },
            "CurrentTemperature": {
                "address": "i10203",
                "scale": 100
            },
            "CurrentHeatingCoolingState": {
                "address": "i4006",
                "map": {
                    "0": 0,
                    "1": 1
                },
                "readonly": true
            }
        }
    ]
}
```
