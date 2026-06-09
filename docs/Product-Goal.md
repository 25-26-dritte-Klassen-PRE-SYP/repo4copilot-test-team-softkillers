# Ist-Zustand

## 1. Wie macht der Kunde heute sein Geschäft? (Business)
Der Kunde nutzt viele separate Apps, Webseiten und Prospekte, um Preise für Tanken, Lebensmittel und Reisen zu vergleichen. Er erstellt manuelle Listen und plant seine Wege selbstständig, ohne eine Verknüpfung der verschiedenen Kategorien.

## 2. Wo ist das Problem / Bedürfnis?
* **Zeitverlust:** Das mühsame Suchen in vielen verschiedenen Apps und Prospekten kostet täglich zu viel Zeit.
* **Intransparenz:** Die tatsächliche Ersparnis ist unklar, da Fahrtkosten und Umwege nicht mit dem Produktpreis gegengerechnet werden.
* **Verpasste Angebote:** Preise für Sprit und Reisen schwanken so schnell, dass man ohne ständige manuelle Prüfung die günstigsten Zeitfenster verpasst.
* **Hohe Spritkosten:** Durch schlechte Planung werden unnötig lange Wege für Besorgungen zurückgelegt, was die Ersparnis beim Einkauf wieder vernichtet.

## Ist-Kontext

### 1. UML Use-Case-Diagramm

#### 1.1 Akteure (Stakeholder-Sicht)
- Nutzer (Endkunde)
- Händler-Systeme (Supermärkte, Tankstellen, Drogerien)
- Reiseplattformen (Flug- und Hotelanbieter)
- Kartendienst / Routing-System
- Smartspend Backend (Systemakteur)

---

#### 1.2 Zentrale Use Cases

##### Nutzer
- Preise vergleichen  
- Einkaufsliste verwalten  
- Route berechnen  
- Preisalarm erhalten  
- Reiseangebote vergleichen  

##### Händler-/Reisesysteme
- Preisdaten bereitstellen  
- Verfügbarkeiten liefern  

##### System
- Daten aggregieren  
- Optimierung durchführen  
- Benachrichtigungen senden  

---

### 2. DFD (Data Flow Diagram)

#### 2.1 Elemente
- Prozess: Smartspend System  
- Terminatoren: Nutzer, externe Systeme  
- Datenflüsse: Preis-, Standort-, Anfrage- und Ergebnisdaten  

---

#### 2.2 Hauptprozesse im System
1. Preisdaten aggregieren  
2. Einkaufsoptimierung berechnen  
3. Route berechnen  
4. Reiseangebote analysieren  
5. Benachrichtigungen auslösen  

---

#### 2.3 Datenflüsse

##### Eingehende Daten (Input)

| Quelle | Daten |
|--------|------|
| Nutzer | Standort, Einkaufsliste, Suchanfragen |
| Preis-APIs | Produktpreise, Händlerdaten |
| Reise-APIs | Flug- & Hotelangebote |
| Routing-API | Karten- und Streckendaten |

##### Ausgehende Daten (Output)

| Ziel | Daten |
|------|------|
| Nutzer | Optimierte Route |
| Nutzer | Günstigste Preise |
| Nutzer | Alerts |
| APIs | Datenanfragen |


# Soll-Kontext

## 1. UML Use-Case-Diagramm (Soll-Zustand)
<img width="1536" height="1024" alt="46f7eeca-7c19-4b83-bb11-3fa627251221" src="https://github.com/user-attachments/assets/9805d48e-94fa-471b-a2e2-d8399a64f190" />

### Akteure

- Nutzer (Endkunde)
- Händler-Systeme (Supermärkte, Tankstellen, Drogerien)
- Reiseplattformen (Flug- und Hotelanbieter)
- Kartendienst / Routing-System
- Smartspend Backend (KI-gestütztes System)
- Zahlungsanbieter (z. B. Cashback, Payment)
- Partner-Apps / Drittanbieter (API-Clients)

---


## 2. DFD (Data Flow Diagram – Soll)
<img width="1536" height="1024" alt="837dcb76-5486-4549-9edf-4bd610b0c469" src="https://github.com/user-attachments/assets/504c5240-386d-483d-8561-3198863ae03d" />

### 2.1 Erweiterte Elemente

- Prozesse:
  - Preis-Service
  - Routing-Service
  - Recommendation-Service
- Datenspeicher:
  - Transaktionsdatenbank
  - Data Warehouse / Analytics
  - Cache (für Echtzeitdaten)
- Event-Broker (z. B. Messaging-System)

---

### 2.2 Datenflüsse

#### Eingehende Daten (Input)

| Quelle | Daten |
|--------|------|
| Nutzer | Standort, Präferenzen, Budget |
| Preis-APIs | Produktpreise, Aktionen |
| Händler | Lagerbestände |
| Reise-APIs | Flug- & Hotelangebote |
| Routing-API | Verkehrsdaten |
| Events | Preisänderungen |

#### Ausgehende Daten (Output)

| Ziel | Daten |
|------|------|
| Nutzer | Optimierte Einkaufsstrategie |
| Nutzer | Dynamische Route |
| Nutzer | Empfehlungen |
| Nutzer | Alerts |
| Partner | API-Daten |
| Analytics | Nutzungsdaten |

---

## 3. API (Soll-Schnittstellen)

### 3.1 API-Endpunkte

#### Authentifizierung
- POST /auth/login
- POST /auth/register
- POST /auth/refresh

#### Benutzer
- GET /user/profile
- PUT /user/profile

#### Shopping
- POST /shopping/optimize
- GET /prices/compare
- POST /alerts

#### Routing
- POST /route/optimize
- GET /route/live

#### Reisen
- GET /travel/search
- POST /travel/optimize

#### Analytics
- GET /user/insights

#### Events
- POST /webhooks
- GET /events/stream

---

### 3.2 Integrationen

- Händler-APIs
- Reiseplattformen
- Routing-/Kartendienste
- Zahlungsanbieter
- Drittanbieter-Apps

---

## 4. UI / UX (Soll-Konzept)
<img width="1536" height="1024" alt="f007693b-7917-4f48-99d8-40067e78834a" src="https://github.com/user-attachments/assets/60feaf02-6f5d-4928-89b8-fbe92fe30184" />


### 4.1 Rollen

- Nutzer
- Power User
- Administrator
- Partner (API-Nutzer)

---

### 4.2 Zentrale Screens

#### Smart Dashboard
- Personalisierte Einsparungen
- Empfehlungen
- Live-Preise

#### Einkaufsplaner
- Automatische Optimierung
- Alternative Vorschläge

#### Routenplaner
- Karte mit optimierter Route
- Echtzeit-Anpassung

#### Reiseplaner
- Kombinierte Angebote
- Preisprognosen

#### Insights & Analytics
- Ausgabenanalyse
- Einsparungen
- Trends

#### Notification Center
- Preisalarme
- Empfehlungen
- Systemmeldungen

---

### 4.3 UX-Prinzipien

- Mobile First
- Echtzeit-Feedback
- Erklärbare KI
- Minimaler Input, maximaler Nutzen
- Personalisierung

# Software-Qualitätsmerkmale

## Änderbarkeit / Wartbarkeit
RELEVANT: Hinzufügen neuer Shops/Anbieter und Updates/Bugfixes

ÜBERPRÜFUNG: 
- Für andere lesbarer Code (evtl. mit externer Person)
- Code-Normen einhalten (Kommentare, Patterns, ...)

### Untermerkmale
- **Analysierbarkeit**  
  RELEVANT: Aufwand zur Diagnose von Fehlern oder zur Identifikation änderungsbedürftiger Teile

- **Modifizierbarkeit**  
  RELEVANT: Aufwand zur Umsetzung von Verbesserungen, Fehlerbehebung oder Anpassungen

- **Stabilität**  
  RELEVANT: Wahrscheinlichkeit unerwarteter Auswirkungen durch Änderungen

- **Testbarkeit / Prüfbarkeit**  
  RELEVANT: Verifikation der geänderten Software

- **Konformität**  
  RELEVANT: Einhaltung von Normen und Vereinbarungen zur Änderbarkeit

---

## Benutzbarkeit
RELEVANT: Benutzer müssen schnell, fehlerfrei und intuitiv mit Shops interagieren

ÜBERPRÜFUNG:
- Besprechung/Workshop mit Kunden

### Untermerkmale
- **Attraktivität**  
  RELEVANT: Visuell ansprechendes Design steigert Nutzerbindung, Vertrauen und langfristige Nutzung

- **Bedienbarkeit**  
  RELEVANT: Einfache, effiziente Bedienung ermöglicht schnelle Nutzung ohne Fehler und Frustration

- **Erlernbarkeit**  
  RELEVANT: Schnelles Verständnis ermöglicht Nutzern sofortige effektive Nutzung ohne Schulungsaufwand (Stichwort Senioren, ...)

- **Verständlichkeit**  
  RELEVANT: Klare Struktur erleichtert das Erfassen von Konzept und Funktionen der Anwendung (Stichwort Senioren, ...)

- **Konformität**  
  RELEVANT: konsistente, vertraute Benutzerführung

---

## Effizienz
RELEVANT: Schnelle Ladezeiten verhindern, dass Benutzer abspringen

ÜBERPRÜFUNG:
- Maximale Ladezeiten festlegen (z.B. Startseite lädt in < 3s)

### Untermerkmale
- **Zeitverhalten**  
  RELEVANT: Schnelle Reaktionen und schnelle Ladezeiten

- **Verbrauchsverhalten**
  NICHT RELEVANT

- **Konformität**  
  RELEVANT: Sicherstellung akzeptablen Zeitverhaltens

---

## Funktionalität
(siehe Issues)
RELEVANT: Korrekte und vollständige Bereitstellung der Funktionen

ÜBERPRÜFUNG:
- Unit-Tests schreiben
- Definition of Done erfüllt
- sonstige Testfälle

### Untermerkmale
- **Angemessenheit**  
  RELEVANT: Korrekte Ergebnisse für Nutzer

- **Richtigkeit**  
  RELEVANT: Funktionen machen genau das Gewünschte

- **Sicherheit**  
  RELEVANT: Schutz vor unberechtigtem Zugriff auf Daten unserer Kunden

- **Interoperabilität**  
  RELEVANT: Zusammenarbeit mit anderen Systemen (z.B. APIs, Banktransaktionen, ...)

- **Ordnungsmäßigkeit**  
  RELEVANT: Einheitlichkeit in Fachlichem und Einhaltung rechtlicher Vorgaben

- **Konformität**  
  RELEVANT: Einhaltung von Standards sorgt für Einheitlichkeit

---

## Übertragbarkeit
RELEVANT: Als App auf verschiedensten Umgebungen installierbar

ÜBERPRÜFUNG:
- läuft auf anderer Umgebung (Android vs. iOS)
- Parallele Zusammenarbeit mit anderen Softwares funktioniert
- Überprüfung der Standards (z.B. bei Protokollen, ...)

### Untermerkmale
- **Anpassbarkeit**  
  RELEVANT: Anpassbar bei neuen Shops, Funktionalitäten, etc.

- **Austauschbarkeit**  
  RELEVANT: Muss austauschbar bleiben, wenn große Änderungen vorgenommen werden

- **Installierbarkeit**  
  RELEVANT: Wenig Aufwand bei Installation für Kunden

- **Koexistenz**  
  RELEVANT: Zusammenarbeit mit APIs (Shopinfos, Aktionen, ...)

- **Konformität**  
  RELEVANT: Standards sorgen für möglichst reibungslosen Ablauf in der Software

---

## Zuverlässigkeit
RELEVANT: stabile, fehlerarme Software, hohe Nutzerzufriedenheit, weniger Abbrüche

### Untermerkmale
- **Reife**  
  RELEVANT: möglichst wenige Abstürze und Fehler, reibungsloser Betrieb

- **Fehlertoleranz**  
  RELEVANT: weiterlaufen trotz Teilausfällen, Vermeidung von Neustarts
  
- **Wiederherstellbarkeit**
  RELEVANT: schnelle System‑ und Datenwiederherstellung, hohe Verfügbarkeit
  
- **Konformität**  
  RELEVANT: Einhaltung von Zuverlässigkeitsstandards, Basis für stabile Betriebsbedingungen

# Ziele

## 1. Zeitersparnis für Nutzer erhöhen
- **Spezifisch:** Reduktion der Zeit für Preisvergleiche und Einkaufsplanung  
- **Messbar:** Von ca. 30 Minuten auf unter 10 Minuten pro Einkauf 


## 2. Effektive Kostenersparnis sichtbar machen
- **Spezifisch:** Anzeige der realen Gesamtersparnis inklusive Fahrtkosten  
- **Messbar:** Mindestens 90 % Genauigkeit bei der Berechnung  


## 3. Preisalarme effizient gestalten
- **Spezifisch:** Echtzeit-Benachrichtigungen bei Preisänderungen  
- **Messbar:** 95 % der Preisänderungen werden innerhalb von 5 Minuten erkannt  


## 4. Spritkosten durch optimierte Routen senken
- **Spezifisch:** Optimierung der Einkaufsrouten zur Reduktion von Fahrwegen  
- **Messbar:** Verringerung der durchschnittlichen Strecke


## 5. Nutzerwachstum erreichen
- **Spezifisch:** Aufbau einer aktiven Nutzerbasis  
- **Messbar:** Beispielsweise 50.000 monatlich aktive Nutzer 


## 6. Datenintegration ausbauen
- **Spezifisch:** Integration von Händler- und Reiseplattformen  
- **Messbar:** 80 % der großen Anbieter in Österreich angebunden  


## 7. Nutzerzufriedenheit steigern
- **Spezifisch:** Verbesserung der User Experience  
- **Messbar:** Durchschnittliche Bewertung von mindestens 4,5 Sternen  


## 8. Mehr Buchungen über die App erreichen 
- **Spezifisch:** Steigerung der Buchungen über die App  
- **Messbar:** 10 % Conversion-Rate bei Reiseangeboten  