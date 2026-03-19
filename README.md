# 🌿 Insane Plant 

Willkommen bei **Insane Plant**! Dies ist eine kompakte 10-Kanal Bewässerungssteuerung auf Basis des ESP8266 (Wemos D1 Mini). Das System erfasst die Bodenfeuchtigkeit von 10 kapazitiven Sensoren über einen Hardware-Multiplexer und steuert 10 unabhängige 5V-Pumpen über zwei Schieberegister und MOSFETs. Alles integriert auf einem Custom-PCB, ausgelegt für eine stabile Spannungsversorgung der Pumpen.

## ✨ Was ist Insane Plant? (Hardware & Features)

* **10-Kanal Bewässerung:** Das System kann bis zu 10 Pflanzen völlig unabhängig voneinander bewässern.
* **Intelligente Sensorauswertung:** Die Bodenfeuchtigkeit wird über 10 kapazitive Sensoren erfasst, die durch einen Hardware-Multiplexer (CD74HC4067) effizient von nur einem analogen Eingang des ESP8266 ausgelesen werden.
* **Leistungsstarke Pumpensteuerung:** 10 unabhängige 5V-Tauchpumpen werden über zwei Schieberegister (SN74HC595) und leistungsstarke N-Channel MOSFETs (AO3400) gesteuert. Die Schaltung ist auf Spitzenlasten von bis zu 3A ausgelegt.
* **Handlötfreundliches SMD-Design:** Die Platine nutzt für passive Bauteile das 1210-Format, was das Handlöten auch für Anfänger deutlich vereinfacht.
* **Vollautomatische Logik in ESPHome:** Das System nutzt ein modulares Konzept in ESPHome. Für jede Pflanze werden automatisch Steuerelemente (Gießmenge, Intervall, Trocken-Schwelle, Kalibrierung) sowie smarte Status-Sensoren (Zeit seit letzter Gießung & Countdown zur nächsten Prüfung) in Home Assistant generiert.
* **Temperaturüberwachung:** Ein optionaler DS18B20 Temperatursensor (One-Wire) kann direkt auf dem Board angeschlossen werden.

  > **💡 Funktionsweise:** Das System liest nacheinander die Feuchtigkeitswerte der 10 Sensoren über den Multiplexer ein. Unterschreitet eine Pflanze die definierte Trocken-Schwelle im gewählten Zeitintervall, triggert der ESP8266 über die Schieberegister automatisch den MOSFET der passenden Pumpe und gießt exakt die eingestellte Milliliter-Menge.

---

## 🛒 Einkaufsliste & Bauteile (BOM)

### 🧠 Mikrocontroller & Logik-ICs
| Bauteil / Bezeichnung | Menge | Beschreibung |
| :--- | :--- | :--- |
| **Wemos D1 Mini** | 1 | Das ESP8266 Mainboard |
| **CD74HC4067** | 1 | 16-Kanal-Multiplexer zum Auslesen der Sensoren (SOIC-24) |
| **SN74HC595** | 2 | Schieberegister zur Steuerung der MOSFETs (SOIC-16) |

### ⚡ Leistungselektronik
| Bauteil / Bezeichnung | Menge | Beschreibung |
| :--- | :--- | :--- |
| **AO3400** | 10 | N-Channel MOSFETs als Pumpentreiber |
| **MP1584** | 1 | Step-Down Konverter Modul (5V Spannungsversorgung) |

### 🧲 Peripherie & Passive Bauteile
| Bauteil / Bezeichnung | Menge | Beschreibung |
| :--- | :--- | :--- |
| **Kapazitiver Sensor** | 10 | Bodenfeuchtesensoren |
| **5V Wasserpumpe** | 10 | Mini-Tauchpumpen für die Bewässerung |
| **DS18B20** | 1 | 1-Wire Temperatursensor |
| **100nF Keramik (1210)**| 5 | Entkopplungskondensatoren für die ICs |

---

## 🛠️ Die Schritt-für-Schritt Anleitung

Jeder kann dieses System bauen. Folge einfach stur diesen Schritten:

### Schritt 1: Hardware fertigen & löten
1. Lade die ZIP-Datei aus dem Ordner `Gerber/` bei einem Platinenfertiger deiner Wahl hoch. Das Board nutzt zwei Lagen (Top/Bottom). Alle Toleranzen sind Standard, du brauchst keine teuren Sonderfertigungs-Optionen wählen.
2. Besorge die nötigen SMD- und THT-Bauteile (siehe BOM).
3. Löte die Bauteile auf. Beginne mit den flachen SMD-Komponenten (1210-Format) und arbeite dich zu den ICs und THT-Bauteilen vor.
4. **Wichtig für die Entkopplung:** Achte darauf, die 100nF Kondensatoren so nah wie möglich an den Pins der ICs (Multiplexer, Schieberegister) zu platzieren.

### Schritt 2: ESPHome vorbereiten
Aus Sicherheitsgründen nutzt dieses Projekt ausgelagerte Passwörter.
1. Erstelle in deinem Home Assistant / ESPHome-Ordner eine neue Datei namens `secrets.yaml`.
2. Füge exakt diese Struktur mit deinen eigenen, echten Daten ein:
   ```yaml
   api_encryption_key: "DEIN_API_KEY"
   ota_password: "DEIN_OTA_PASSWORD"
   wifi_ssid: "DEIN_WLAN_NAME"
   wifi_password: "DEIN_WLAN_PASSWORT"
   ap_password: "DEIN_FALLBACK_HOTSPOT_PASSWORT"
   ```
3. Kopiere die Hauptdatei `insane-plant.yaml` sowie die Modul-Schablone `plant_module.yaml` aus dem Ordner `ESPHome/` in deinen ESPHome-Konfigurationsordner.
4. Binde den Wemos D1 Mini in dein ESPHome Dashboard ein.
5. Flashe das Board. Das System generiert nun automatisch alle Steuerelemente in Home Assistant.

### Schritt 3: Verkabelung & Erster Start
1. **Sensoren:** Schließe deine kapazitiven Bodenfeuchtesensoren an die entsprechenden Eingänge an.
2. **Pumpen:** Verbinde deine 10 Wasserpumpen (5V) mit den MOSFET-Ausgängen.
3. **Temperatur (Optional):** Schließe den DS18B20 Temperatursensor (One-Wire) an.
4. **Power On:** Verbinde das 5V Netzteil mit der Platine.

---

## ⚖️ Lizenz
Dieses komplette Projekt steht unter der [CC BY-NC-SA 4.0 Lizenz](https://creativecommons.org/licenses/by-nc-sa/4.0/). 
Das bedeutet: Nachbauen und Anpassen für private Zwecke ist ausdrücklich erwünscht, jede kommerzielle Nutzung oder der Verkauf sind strikt verboten!

---

## ☕ Support dieses Projekts
Wenn dir das System gefällt und du meine Arbeit unterstützen möchtest, freue ich mich riesig über einen virtuellen Kaffee!

<a href="https://www.paypal.me/babeinlovexd">
  <img src="https://img.shields.io/badge/Donate-PayPal-blue.svg?style=for-the-badge&logo=paypal" alt="Donate mit PayPal">
</a>

Jeder Cent fließt direkt in neue Projekte und Prototypen! 🚀
---

## 👨‍💻 Entwickelt von

| [<img src="https://avatars.githubusercontent.com/u/43302033?v=4" width="100"><br><sub>**Christopher**</sub>](https://github.com/babeinlovexd) |
| :---: |

---
