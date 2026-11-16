<p align="center">
  <a href="https://github.com/babeinlovexd"><img src="https://raw.githubusercontent.com/babeinlovexd/babeinlovexd/main/bxd-github-header.svg" width="520" alt="BxD / BabeinlovexD"></a>
</p>

# Insane Plants

[English](README_en.md) · [Fragen & Fehler](https://github.com/babeinlovexd/Insane-Plants/issues) · [Releases](https://github.com/babeinlovexd/Insane-Plants/releases)

Modulare **Bewässerungssteuerung für bis zu zehn Pflanzen** mit ESP8266 und ESPHome. Ein CD74HC4067 liest die Feuchtesensoren aus; zwei SN74HC595 und AO3400-MOSFETs steuern die Pumpen.

<p align="center"><img src="ip.png" alt="Insane Plants" width="300"></p>

## Funktionen

- Feuchteschwelle, Gießmenge, Pumpenkalibrierung und Prüfintervall je Pflanze.
- Zeitbasierte Dosierung aus gemessenem Pumpendurchfluss und globalem Saisonfaktor.
- Wassertank-Eingang, der vor einem automatischen Gießvorgang geprüft wird.
- Zehn-Sekunden-Kalibriertest und eine Pumpenabschaltung nach 60 Sekunden in der GPIO-Schalterautomation.
- Optionaler DS18B20 sowie Anzeigen für letzte Gießung und nächste Prüfung.

## Hardware & Dateien

| Bauteil | Anzahl |
| :--- | :--- |
| Wemos D1 Mini / ESP8266 | 1 |
| CD74HC4067 | 1 |
| SN74HC595 | 2 |
| AO3400-Pumpentreiber | Bis zu 10 |
| Kapazitive Feuchtesensoren und 5-V-Pumpen | Je bis zu 10 |
| DS18B20 | Optional |

Fertigungsdaten: [Gerber](Gerber). Konfiguration: [ESPHome/insane-plant.yaml](ESPHome/insane-plant.yaml) und [ESPHome/plant_module.yaml](ESPHome/plant_module.yaml). Die Haupt-YAML dokumentiert Pins und Kanalzuordnungen.

## Installation

1. **Beide YAML-Dateien** in denselben ESPHome-Konfigurationsordner kopieren.
2. Eine eigene `secrets.yaml` mit `api_encryption_key`, `ota_password`, `wifi_ssid`, `wifi_password` und `ap_password` anlegen.
3. Pin-Substitutionen und Pflanzenpakete prüfen. Nicht benötigte Pflanzen unter `packages` deaktivieren.
4. Sensoren, Pumpentreiber, Versorgung und Tankschalter passend zur Konfiguration anschließen. Der Tankeingang liegt in diesem Aufbau auf GPIO16.
5. In ESPHome validieren und kompilieren, den D1 Mini flashen und in Home Assistant hinzufügen.

## Kalibrierung & Bedienung

1. Die Fördermenge jeder Pumpe beim Zehn-Sekunden-Test messen und als **ml pro 10 s** eintragen.
2. Zielmenge, Feuchteschwelle und Prüfintervall je Pflanze einstellen.
3. Feuchtesensoren gegen die eigenen Trocken-/Nasswerte kalibrieren. Die linearen Vorgaben in `plant_module.yaml` müssen zum tatsächlichen Sensor passen.
4. Mit Saisonfaktor **1,0** beginnen. Die Laufzeit berechnet sich als `Menge / Kalibrierung × 10 s × Saisonfaktor`.
5. Tankschalter testen und die ersten vollständigen Gießvorgänge beaufsichtigen.

Das System besitzt keinen Durchflussmesser: Die Menge wird aus der kalibrierten Laufzeit geschätzt. Die 60-Sekunden-Abschaltung kann einen länger berechneten Vorgang verkürzen; wähle passende Einstellungen. Der Tanksensor wird am Start des Gießskripts geprüft, nicht dort fortlaufend während der Dosis überwacht.

## Fehlersuche

- **Es wird nicht gegossen:** Tankzustand, positive Kalibrierung/Menge, Feuchteschwelle und abgelaufenes Prüfintervall prüfen.
- **Falsche Menge:** Jede Pumpe mit tatsächlicher Schlauchlänge und Förderhöhe kalibrieren, Saisonfaktor und Laufzeitbegrenzung prüfen.
- **Falsche Pflanze/Pumpe:** `mux_pin` und `sr_pin` der Pakete mit der Verkabelung abgleichen.
- **Entitäten fehlen:** `plant_module.yaml` neben der Hauptdatei ablegen und aktivierte Pakete prüfen.

## Lizenz

[CC BY-NC-SA 4.0](LICENSE). Es gilt die bestehende Projektlizenz. Drittanbieter-Komponenten behalten ihre jeweiligen Lizenzen.

---

## Builds und Releases

Version: **1.4.1** ([VERSION](VERSION)). [GitHub Actions](.github/workflows/release.yml) prüft Pushes und Pull Requests. Ein Tag passend zu `v` + `VERSION` veröffentlicht das Release erst nach erfolgreichen Jobs; manuelle Läufe liefern Build-Artefakte. [Downloads](https://github.com/babeinlovexd/Insane-Plants/releases).

ESPHome-CI verwendet Platzhalter-Zugangsdaten und flasht keine Geräte. Vor dem Einsatz eigene Secrets und Pinbelegung prüfen.

## Mitmachen

Fragen, Fehlerberichte und Beiträge sind willkommen. Nenne Version, Aufbau und Schritte zum Nachstellen im [Issue](https://github.com/babeinlovexd/Insane-Plants/issues). Größere Änderungen besprechen wir am besten vor einem Pull Request.

Dieses Hobbyprojekt unterstützen: [PayPal](https://www.paypal.me/babeinlovexd).

[BxD / BabeinlovexD](https://github.com/babeinlovexd) · [Website](https://babeinlovexd.de/)
