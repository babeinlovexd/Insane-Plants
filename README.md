# 🌿 Insane Plant 

Willkommen bei **Insane Plant**! Dies ist eine kompakte 10-Kanal Bewässerungssteuerung auf Basis des ESP8266 (Wemos D1 Mini). Das System erfasst die Bodenfeuchtigkeit von 10 kapazitiven Sensoren über einen Hardware-Multiplexer und steuert 10 unabhängige 5V-Pumpen über ein Schieberegister und MOSFETs. Alles integriert auf einem Custom-PCB, ausgelegt für eine stabile Spannungsversorgung der Pumpen.

## 📦 Verzeichnis-Inhalt
* `/Gerber/` - Die Produktionsdaten für die Platine (ZIP-Datei).
* `/ESPHome/` - Die Konfigurationsdateien für das ESP8266 Mainboard (`insane-pant-v1.yaml` und `plant_module.yaml`).

---

## 🛠️ Schritt 1: Die Platine fertigen lassen
Lade die ZIP-Datei aus dem Ordner `/Gerber/` bei einem Platinenfertiger deiner Wahl hoch. Das Board nutzt zwei Lagen (Top/Bottom). Alle Toleranzen sind Standard, du brauchst keine teuren Sonderfertigungs-Optionen wählen.

## 🛒 Schritt 2: Bauteile besorgen & Bestückung
Die nötigen SMD- und THT-Bauteile bekommst du problemlos online. Eine genaue Stückliste findest du weiter unten.

**Wichtige Löthinweise:**
1. **SMD-Größen:** Die passiven Bauteile sind im 1210-Format gehalten, was das Handlöten deutlich vereinfacht.
2. **Entkopplung:** Achte darauf, die 100nF Kondensatoren so nah wie möglich an den Pins der ICs (Multiplexer, Schieberegister) zu platzieren.

---

## 💻 Schritt 3: Software (ESPHome)
Der Wemos D1 Mini übernimmt die Sensor-Auswertung, die Ansteuerung der Schieberegister und die Logik für das automatische Gießen. Das System nutzt ein modulares Konzept mit einer Hauptkonfiguration und einer Schablone für jede einzelne Pflanze.

1. Binde den ESP8266 in dein **ESPHome** Dashboard ein.
2. Kopiere die Hauptdatei `insane-plant.yaml` sowie die Modul-Schablone `plant_module.yaml` in deinen ESPHome-Konfigurationsordner.
3. Passe deine Zugangsdaten in deiner `secrets.yaml` an. Das Projekt benötigt folgende Parameter: `api_encryption_key`, `ota_password`, `wifi_ssid`, `wifi_password` und `ap_password`.
4. Flashe das Board. Das System generiert nun automatisch alle Steuerelemente (Gießmenge, Intervall, Trocken-Schwelle, Kalibrierung) für alle 10 Pflanzen übersichtlich in Home Assistant!

---

## 🔌 Schritt 4: Verkabelung & Erster Start
1. **Sensoren:** Schließe deine kapazitiven Bodenfeuchtesensoren an die entsprechenden Eingänge an.
2. **Pumpen:** Verbinde deine 10 Wasserpumpen (5V) mit den MOSFET-Ausgängen.
3. **Temperatur (Optional):** Schließe den DS18B20 Temperatursensor (One-Wire) an.
4. **Power On:** Verbinde das 5V Netzteil mit der Platine. 

*Funktionsweise: Das System liest nacheinander die Feuchtigkeitswerte der 10 Sensoren über den CD74HC4067 Multiplexer ein. Unterschreitet eine Pflanze die definierte Trocken-Schwelle im gewählten Zeitintervall, triggert der ESP8266 über das 74HC595 Schieberegister automatisch den MOSFET der passenden Pumpe und gießt exakt die eingestellte Milliliter-Menge. Die Schaltung ist dabei auf Spitzenlasten von bis zu 3A ausgelegt.*

---

## 🛒 Stückliste (BOM - Bill of Materials)

### 🧠 Mikrocontroller & Logik-ICs
| Bauteil / Bezeichnung | Menge | Beschreibung |
| :--- | :--- | :--- |
| **Wemos D1 Mini** | 1 | Das ESP8266 Mainboard |
| **CD74HC4067** | 1 | 16-Kanal-Multiplexer zum Auslesen der Sensoren (SOIC-24) |
| **SN74HC595** | 1 | Schieberegister zur Steuerung der MOSFETs (SOIC-16) |

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

## ⚖️ Lizenz
Dieses komplette Projekt steht unter der [CC BY-NC-SA 4.0 Lizenz](https://creativecommons.org/licenses/by-nc-sa/4.0/). 
Das bedeutet: Nachbauen und Anpassen für private Zwecke ist ausdrücklich erwünscht, jede kommerzielle Nutzung oder der Verkauf sind strikt verboten!
