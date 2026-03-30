# Intelligent Traffic Perception System (ITPS) Straßen-Erfassung (Zählstelle mit Klassierung)

---

# 1 Idee

Ziel ist die automatisierte Erfassung von Verkehrsströmen entlang eines Straßenabschnitts mittels einer fest installierten Kamera. Die Kamera soll an einer bestehenden Strassenbeleuchtung montiert werden können, die Stromversorgung soll direkt über eine Nachladung von der Strassenbeleuchtung erfolgen.

Das System dient als flexible Alternative zu klassischen Zählstellen und ermöglicht:

- Fahrzeugzählung
- Klassifikation nach Swiss-10
- Richtungsbestimmung
- Spurbezogene Auswertung

---

# 2 Zielsetzung

Das System soll:

- zuverlässig Fahrzeuge erkennen und zählen
- diese nach Swiss-10 klassifizieren
- die Fahrtrichtung bestimmen
- robuste Ergebnisse bei unterschiedlichen Umweltbedingungen liefern

---

# 3 Grundprinzip

Die Erfassungsstelle arbeitet autonom:

1. Kamera erfasst kontinuierlich die Szene
2. Fahrzeuge werden lokal erkannt
3. Tracking bestimmt Bewegungsrichtung
4. Klassifikation erfolgt pro Fahrzeug
5. Ergebnisse werden aggregiert und gespeichert

---

# 4 Systemarchitektur

### Edge-Komponenten

- Kamera (fix installiert)
- AI-Beschleuniger (z. B. Hailo)
- lokaler Rechner (z. B. Raspberry Pi CM5)

### Zentrale Plattform

- Datenspeicherung
- Modelltraining
- Modellverwaltung

---

# 5 Funktionsweise

## 5.1 Detektion & Klassifikation

- Fahrzeuge werden mittels eines YOLO-Modells erkannt
- Klassifikation erfolgt nach Swiss-10
- nur relevante Klassen werden berücksichtigt und können selektiv gewählt werden

---

## 5.2 Tracking & Richtungsbestimmung

- Fahrzeuge werden über mehrere Frames verfolgt
- aus der Bewegung wird die Fahrtrichtung bestimmt
- optional: Zuordnung zu definierten Spuren gem. Konfiguration

---

## 5.3 Zählung

- Fahrzeuge werden gezählt, sobald sie definierte Zonen oder Linien passieren
- Mehrfachzählungen werden durch Tracking verhindert

---

# 6 Datenstrategie

Es werden gezielt Daten für das Training gesammelt:

- unsichere Klassifikationen
- zufällige Beispiele zur Sicherstellung der Diversität

---

# 7 Lernstrategie

Die einzelnen Erfassungsstellen erfassen den Kamerafeed und werten die Kamerabilder direkt über ein ML-Modell aus. Durch einen ML-Algorithmus wie YOLO können di einzelnen Fahrzeuge erkannt und getrackt werden. 
Damit die Erfassung und Auswertung der Bilder kontinuierlich verbessert werden kann, soll ein stetiger Verbesserungsprozess der genutzten Modelle angestrebt werden.

Dies könnte folgendermassen aussehen:

1. ausgewählte Bilder werden gespeichert (Grenzfälle und Durchschnittswerte)
2. manuelle Klassifikation erfolgt zentral, manuell
3. Datensätze werden erweitert
4. Modell wird feinjustiert (ab einigen 1000 neuen Datensätzen)
5. Neues Modell wird validiert mit neuen Daten und Aufnahmen
6. verbessertes Modell wird zurückgespielt

---

# 8 Modellkonzept

**Globales Modell** 

Ein einheitliches, globales Modell wird aufgrund aller aufgenommenen Bilder weiterentwickelt. Das Basismodell von bspw. YOLO wird dadurch mit den unterschiedlichen Kamerawinkel weiter verbessert und auf die Anwendung optimiert. 

Dieses Modell wird auf jeder neuen Erfassungsstelle ausgerollt.

**Lokales Modell**

Lokal können die vorhandenen Modelle weiterentwickelt werden. Die Erfassungsstelle speichert Bilder unterhalb einer bestimmten Sicherheit für die manuelle Klassierung ab. Um einen edge-bias zu verhindern, werden zusätzlich durchschnittliche Bilder randomisiert gespeichert. 

Sobald diese Bilder manuell klassiert werden, kann ein Modell spezifisch für diese Erfassungsstelle optimiert werden. Dies kann bei speziellen Blickwinkel oder Strassengegebenheiten einen wesentlichen Vorteil bringen. 
Das neue Modell wird validiert und anschliessend auf die Erfassungsstelle geladen. 

---

# 9 Technische Umsetzung (konzeptionell)

- Kamera liefert kontinuierlichen Stream, ca. 16MPx
- YOLO läuft auf AI-Beschleuniger
- Tracking erfolgt lokal
- Ergebnisse werden aggregiert
- selektive Datenübertragung an zentrale Infrastruktur

Sobald eine Erfassungsstelle die gewünschte Genauigkeit erreicht hat, könnte die Datenverbindung optional über NB-IOT erfolgen, also LoRaWAN.

---

# 10 Erweiterungsmöglichkeiten

- Feinere Spurauswertung wie Fahrräder auf Trottoir
- Erkennung Spezialfahrzeuge wie Traktor, Baumaschinen, Blaulichtfahrzeuge, usw.
- zusätzliche Sensorintegration
- Kombination mehrerer Kameras
- Echtzeit-Dashboards
- Integration Stauerkennung
- Erweiterte Analyse zum Verhalten von Fahrzeugen

---
