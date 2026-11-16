<p align="center">
  <a href="https://github.com/babeinlovexd"><img src="https://raw.githubusercontent.com/babeinlovexd/babeinlovexd/main/bxd-github-header.svg" width="520" alt="BxD / BabeinlovexD"></a>
</p>

# Insane Plants

[Deutsch](README.md) · [Issues](https://github.com/babeinlovexd/Insane-Plants/issues) · [Releases](https://github.com/babeinlovexd/Insane-Plants/releases)

A modular **ESP8266 / ESPHome irrigation controller for up to ten plants**. A CD74HC4067 multiplexes moisture sensors; two SN74HC595 shift registers and AO3400 MOSFETs control the pumps.

<p align="center"><img src="ip.png" alt="Insane Plants" width="300"></p>

## Features

- Independent moisture thresholds, watering amounts, pump calibration and check intervals for each plant.
- Time-based dosing using measured pump flow and a global seasonal multiplier.
- Tank-empty input checked before an automatic watering run.
- Ten-second calibration button and a maximum pump on-time of 60 seconds in the GPIO switch automation.
- Optional DS18B20 temperature input; time-since-watering and next-check sensors.

## Hardware & files

| Part | Quantity |
| :--- | :--- |
| Wemos D1 Mini / ESP8266 | 1 |
| CD74HC4067 | 1 |
| SN74HC595 | 2 |
| AO3400 pump drivers | Up to 10 |
| Capacitive moisture sensors and 5 V pumps | Up to 10 each |
| DS18B20 | Optional |

Manufacturing files: [Gerber](Gerber). Configuration: [ESPHome/insane-plant.yaml](ESPHome/insane-plant.yaml) and [ESPHome/plant_module.yaml](ESPHome/plant_module.yaml). The main YAML documents controller pins and assigns pump/multiplexer channels.

## Installation

1. Copy **both YAML files** into the same ESPHome configuration directory.
2. Create your own `secrets.yaml` with `api_encryption_key`, `ota_password`, `wifi_ssid`, `wifi_password` and `ap_password`.
3. Check the pin substitutions and enabled plant packages. Disable unused plant entries under `packages`.
4. Wire sensors, pump drivers, power and the tank switch to match the configuration. The tank input is on GPIO16 in this setup.
5. Validate and compile in ESPHome, flash the D1 Mini and add it to Home Assistant.

## Calibration & operation

1. Measure a pump's delivery during the ten-second test and enter the result as **ml per 10 s**.
2. Set target amount, moisture threshold and checking interval for each plant.
3. Check moisture calibration against dry/wet readings for the actual sensors. The supplied linear values in `plant_module.yaml` are starting values to review, not a calibration for every sensor.
4. Start with seasonal multiplier **1.0**. The calculated duration is `amount / calibration × 10 s × seasonal multiplier`.
5. Test the tank switch and supervise the first full cycle.

There is no flow meter: amounts are estimates from calibrated runtime. The 60-second cutoff can shorten a long requested run; choose settings that fit within it. The tank input is checked at the start of the watering script; continuous tank monitoring during a running dose is not implemented there.

## Troubleshooting

- **No watering:** Check tank-empty state, positive calibration/amount, moisture threshold and elapsed checking interval.
- **Incorrect amount:** Recalibrate each pump with the actual tubing and height, and check the seasonal multiplier/runtime cutoff.
- **Wrong plant/pump:** Compare package `mux_pin` and `sr_pin` assignments to the wiring.
- **Missing entities:** Ensure `plant_module.yaml` is beside the main file and enabled packages are valid.

## License

[CC BY-NC-SA 4.0](LICENSE). The existing project license applies; third-party components retain their own licenses.

---

## Builds and releases

Version: **1.4.1** ([VERSION](VERSION)). [GitHub Actions](.github/workflows/release.yml) checks each push and pull request. A tag matching `v` + `VERSION` publishes a release after all jobs succeed; manual runs provide build artifacts. [Downloads](https://github.com/babeinlovexd/Insane-Plants/releases).

ESPHome CI uses dummy credentials and does not flash devices. Configure your own secrets and pin assignments before deployment.

## Contributing

Questions, bug reports and contributions are welcome. Include version, setup and reproduction steps in an [Issue](https://github.com/babeinlovexd/Insane-Plants/issues). Please discuss larger changes before a pull request.

Support this hobby project: [PayPal](https://www.paypal.me/babeinlovexd).

[BxD / BabeinlovexD](https://github.com/babeinlovexd) · [Website](https://babeinlovexd.de/)
