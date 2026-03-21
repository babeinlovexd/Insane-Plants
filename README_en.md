# 🌿 Insane Plant

[🇩🇪 Deutsche Version](README.md)

Welcome to **Insane Plant**! This is a compact 10-channel irrigation controller based on the ESP8266 (Wemos D1 Mini). The system captures soil moisture from 10 capacitive sensors via a hardware multiplexer and controls 10 independent 5V pumps using two shift registers and MOSFETs. Everything is integrated onto a custom PCB, designed for a stable power supply to the pumps.

## ✨ What is Insane Plant? (Hardware & Features)

* **10-Channel Irrigation:** The system can water up to 10 plants completely independently of each other.
* **Intelligent Sensor Evaluation:** Soil moisture is recorded via 10 capacitive sensors, efficiently read from just one analog input of the ESP8266 using a hardware multiplexer (CD74HC4067).
* **Powerful Pump Control:** 10 independent 5V submersible pumps are controlled via two shift registers (SN74HC595) and powerful N-channel MOSFETs (AO3400). The circuit is designed for peak loads of up to 3A.
* **Hand-Soldering Friendly SMD Design:** The board uses the 1210 format for passive components, significantly simplifying hand soldering, even for beginners.
* **Fully Automatic Logic in ESPHome:** The system utilizes a modular concept in ESPHome. For each plant, controls (water amount, interval, dry threshold, calibration) and smart status sensors (time since last watering & countdown to next check) are automatically generated in Home Assistant.
* **Temperature Monitoring:** An optional DS18B20 temperature sensor (One-Wire) can be connected directly to the board.
* **Global Season Scaling:** A global multiplier allows you to adjust the watering amount for all plants simultaneously according to the season (e.g. summer/winter), without having to change each plant individually.
* **Scalable System:** Do you need fewer than 10 plants? The system is designed so that you can simply comment out unneeded plants in the configuration to save components and ESP resources.

  > **💡 How it works:** The system sequentially reads the moisture values of the 10 sensors via the multiplexer. If a plant falls below the defined dry threshold within the selected time interval, the ESP8266 automatically triggers the MOSFET of the corresponding pump via the shift registers and waters exactly the set milliliter amount.

---

## 🛒 Bill of Materials (BOM)

### 🧠 Microcontroller & Logic ICs
| Component / Designation | Quantity | Description |
| :--- | :--- | :--- |
| **Wemos D1 Mini** | 1 | The ESP8266 Mainboard |
| **CD74HC4067** | 1 | 16-Channel Multiplexer for reading sensors (SOIC-24) |
| **SN74HC595** | 2 | Shift registers to control MOSFETs (SOIC-16) |

### ⚡ Power Electronics
| Component / Designation | Quantity | Description |
| :--- | :--- | :--- |
| **AO3400** | 10 | N-Channel MOSFETs as pump drivers |
| **MP1584** | 1 | Step-Down Converter Module (5V power supply) |

### 🧲 Peripherals & Passive Components
| Component / Designation | Quantity | Description |
| :--- | :--- | :--- |
| **Capacitive Sensor** | 10 | Soil moisture sensors |
| **5V Water Pump** | 10 | Mini submersible pumps for watering |
| **DS18B20** | 1 | 1-Wire temperature sensor |
| **100nF Ceramic (1210)**| 5 | Decoupling capacitors for ICs |

---

## 🛠️ Step-by-Step Instructions

Anyone can build this system. Just follow these steps strictly:

### Step 1: Manufacture & Solder Hardware
1. Upload the ZIP file from the `Gerber/` folder to a PCB manufacturer of your choice. The board uses two layers (Top/Bottom). All tolerances are standard; no expensive custom manufacturing options are needed.
2. Get the necessary SMD and THT components (see BOM).
3. Solder the components on. Start with the flat SMD components (1210 format) and work your way up to the ICs and THT components.
4. **Important for decoupling:** Make sure to place the 100nF capacitors as close as possible to the pins of the ICs (multiplexer, shift registers).

### Step 2: Prepare ESPHome
For security reasons, this project uses externalized passwords.
1. Create a new file named `secrets.yaml` in your Home Assistant / ESPHome folder.
2. Insert exactly this structure with your own, real data:
   ```yaml
   api_encryption_key: "YOUR_API_KEY"
   ota_password: "YOUR_OTA_PASSWORD"
   wifi_ssid: "YOUR_WIFI_NAME"
   wifi_password: "YOUR_WIFI_PASSWORD"
   ap_password: "YOUR_FALLBACK_HOTSPOT_PASSWORD"
   ```
3. Copy the main file `insane-plant.yaml` as well as the module template `plant_module.yaml` from the `ESPHome/` folder into your ESPHome configuration folder.

   > **💡 Setup Tip:** If you want to water fewer than 10 plants, you can easily comment out the unneeded plants in the `insane-plant.yaml` file under the `packages:` section by adding a `#` in front of them.
4. Add the Wemos D1 Mini to your ESPHome Dashboard.
5. Flash the board. The system will now automatically generate all controls in Home Assistant.

### Step 3: Wiring & First Start
1. **Sensors:** Connect your capacitive soil moisture sensors to the corresponding inputs.
2. **Pumps:** Connect your 10 water pumps (5V) to the MOSFET outputs.
3. **Temperature (Optional):** Connect the DS18B20 temperature sensor (One-Wire).
4. **Power On:** Connect the 5V power supply to the board.

---

## ⚖️ License
This entire project is licensed under the [CC BY-NC-SA 4.0 License](https://creativecommons.org/licenses/by-nc-sa/4.0/).
This means: Rebuilding and modifying for private purposes is explicitly desired; any commercial use or sale is strictly prohibited!

---

## ☕ Support this Project
If you like the system and want to support my work, I would be absolutely thrilled about a virtual coffee!

<a href="https://www.paypal.me/babeinlovexd">
  <img src="https://img.shields.io/badge/Donate-PayPal-blue.svg?style=for-the-badge&logo=paypal" alt="Donate with PayPal">
</a>

Every cent flows directly into new projects and prototypes! 🚀
---

## 👨‍💻 Developed by

| [<img src="https://avatars.githubusercontent.com/u/43302033?v=4" width="100"><br><sub>**Christopher**</sub>](https://github.com/babeinlovexd) |
| :---: |

---
