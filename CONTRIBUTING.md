# 🛠️ Contributing zu Insane Plant 

Danke für dein Interesse, zu diesem Projekt beizutragen. Egal ob Bugfix in der ESPHome-Konfiguration oder Verbesserungen am Platinen-Layout – Beteiligung ist willkommen. 

Bitte beachte für eine effiziente Zusammenarbeit die folgenden Richtlinien.

## 🐛 Fehler melden (Issues)
Wenn ein Fehler auftritt (z. B. bei der Pumpensteuerung oder den Sensorwerten), nutze den **Issues**-Reiter. 
* **Vorab prüfen:** Existiert das Issue bereits?
* **Präzise Beschreibung:** Was genau ist passiert? Welche ESPHome-Version läuft? Welches 5V-Netzteil ist im Einsatz?
* **Logs:** Bei Software-Problemen immer die entsprechenden Log-Ausgaben aus ESPHome anhängen.

## 💡 Feature-Requests
Für Ideen zu Erweiterungen (z. B. andere Sensoren, Displays):
* Bitte öffne dafür **kein** Issue.
* Nutze stattdessen die **GitHub Discussions** für das Brainstorming zur technischen Machbarkeit vor der eigentlichen Umsetzung.

## 🚀 Pull Requests (PRs) einreichen
Für Code- oder Hardware-Änderungen stelle bitte einen Pull Request. 

### Software (ESPHome):
1. **Test-Pflicht:** Änderungen müssen zwingend auf der V1-Hardware live getestet worden sein.
2. **Code-Stil:** Behalte die saubere YAML-Struktur bei.
3. **Begründung:** Beschreibe im PR exakt das gelöste Problem oder die neu hinzugefügte Funktion.

### Hardware (Gerber / CAD-Dateien):
1. **Technische Begründung:** Erkläre bei Änderungen an Leiterbahnen oder Komponenten den genauen elektrotechnischen Nutzen.
2. **Verfügbarkeit:** Neue Bauteile in der BOM müssen Standard-Komponenten im 1210-Format und online leicht bestellbar sein.
3. **Design-Regeln:** Änderungen am 5V-Power-Routing werden streng geprüft. Die 1,34 mm Leiterbahnbreite für die Hauptstromversorgung (ausgelegt auf 3A Peak) und die 0,25 mm Clearance (insbesondere die Verjüngung an den IC-Pins) müssen zwingend eingehalten werden.
