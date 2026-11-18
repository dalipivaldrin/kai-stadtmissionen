# Kai – Stadtmissionen 🏙️

Ein 2D-Jump'n'Run im Browser, gebaut mit reinem **HTML5 Canvas und JavaScript** – ohne Framework, ohne Bilder, ohne externe Abhängigkeiten. Alles (Figur, Gegner, Hintergründe, Sounds) wird direkt im Code erzeugt.

**▶ Jetzt spielen:** https://dalipivaldrin.github.io/kai-stadtmissionen/

---

## Die Idee

Ich wollte ein Plattform-Spiel im Stil der klassischen Jump'n'Runs machen – aber mit einer **eigenen Figur und eigener Spielidee**, nicht als Kopie von Super Mario.

Daraus entstand **Kai**, ein Stadtkurier mit türkisem Hoodie, Kopfhörern und Rucksack. Statt einfach nur ans Levelende zu rennen, muss Kai in jedem Level eine **Mission** erfüllen. Erst wenn die Mission erledigt ist, öffnet sich das Ziel.

| Mission | Thema | Aufgabe | Ziel |
|---|---|---|---|
| 1 – Der Paketbote | Stadt am Tag | 5 verstreute Pakete einsammeln | Briefkasten |
| 2 – Stromausfall | Fabrik in der Abenddämmerung | 5 Notstrom-Schalter umlegen | Fabriktür |
| 3 – Katzenrettung | Dächer in der Nacht | 5 Kätzchen retten (sie laufen Kai hinterher) | Tierheim |

## Steuerung

| Aktion | Tastatur | Handy |
|---|---|---|
| Laufen | ← → oder A / D | ◀ ▶ |
| Springen | ↑, W oder Leertaste | ▲ |
| Pause | P oder Esc | – |
| Bestätigen | Enter / Leertaste | Bildschirm antippen |

**Tipps:** Auf Roboter und Drohnen springen schaltet sie aus. Hält man beim Draufspringen die Sprungtaste, springt Kai höher ab. Kontrollpunkt-Fahnen speichern den Fortschritt.

---

## Wie ich vorgegangen bin

### 1. Konzept festlegen
Bevor ich Code geschrieben habe, habe ich festgelegt:
- **Genre:** Side-Scroller-Plattformer mit Missionen
- **Hauptfigur:** eigene Figur (Kai), klar unterscheidbar von bekannten Spielfiguren
- **Umfang:** 3 Levels mit steigender Schwierigkeit, je eine eigene Mission und ein eigenes Farbthema
- **Technik:** eine einzige `index.html`, damit das Spiel überall ohne Installation läuft

### 2. Technische Grundstruktur
Das Spiel basiert auf einem klassischen **Game Loop**:
- `update()` berechnet Bewegung, Kollisionen und Spiellogik
- `draw()` zeichnet alles auf das Canvas
- Ein **fester Zeitschritt** (60 Updates pro Sekunde) sorgt dafür, dass das Spiel auf schnellen und langsamen Bildschirmen gleich schnell läuft

Das Spiel hat verschiedene **Zustände** (`title`, `intro`, `play`, `pause`, `clear`, `gameover`, `win`), zwischen denen gewechselt wird.

### 3. Levels als Kacheln (Tilemap)
Die Welt ist ein Raster aus 32×32-Pixel-Kacheln (16 Zeilen hoch). Statt jedes Level Zeichen für Zeichen zu tippen, habe ich kleine **Bau-Funktionen** geschrieben:

```js
L.ground(0, 25);     // Boden von Spalte 0 bis 25
L.plat(11, 12, 16);  // Kistenplattform in Zeile 11
L.put('M', 10, 14);  // Missionsobjekt platzieren
L.put('G', 13, 22);  // Bodenroboter platzieren
```

So lassen sich Levels schnell anpassen. Beim Levelaufbau habe ich darauf geachtet, dass Plattformen höchstens 3 Kacheln übereinander liegen und Lücken höchstens 4 Kacheln breit sind – das entspricht der maximalen Sprunghöhe und -weite von Kai.

### 4. Physik & Steuerung
Damit sich das Springen gut anfühlt, habe ich ein paar Techniken aus professionellen Plattformern eingebaut:
- **Beschleunigung & Reibung** statt sofortiger Geschwindigkeit
- **Variable Sprunghöhe:** Taste kurz drücken = kleiner Sprung, lang halten = hoher Sprung
- **Coyote Time:** man kann noch kurz springen, nachdem man von einer Kante gelaufen ist
- **Jump Buffer:** ein Sprung, der kurz vor der Landung gedrückt wird, wird trotzdem ausgeführt

Die Kollision läuft getrennt nach **X- und Y-Achse** gegen die Kacheln (AABB-Kollision).

### 5. Gegner
- **Schrott-Bot:** fährt am Boden hin und her und dreht an Wänden, Kanten und Stacheln um
- **Drohne:** fliegt in einer Wellenbewegung hin und her

Landet Kai von oben auf einem Gegner, wird dieser ausgeschaltet. Berührt er ihn seitlich, verliert er ein Leben und wird kurz unverwundbar.

### 6. Grafik ohne Bilddateien
Alle Grafiken werden mit Canvas-Formen gezeichnet: Kai, Roboter, Drohnen, Pakete, Schalter, Kätzchen, Gebäude. Die Hintergründe haben zwei **Parallax-Ebenen** (weiter entfernte Häuser bewegen sich langsamer), die mit einem Zufallsgenerator mit festem Startwert erzeugt werden, damit jedes Level immer gleich aussieht.

### 7. Sound
Die Soundeffekte (Sprung, Münze, Treffer, Mission erfüllt …) werden mit der **Web Audio API** live erzeugt – kurze Töne mit Oszillatoren, ganz ohne Audiodateien.

### 8. Missionen, Punkte & Fortschritt
- Fortschritt der Mission wird oben in der Leiste angezeigt (z. B. `Pakete: 3 / 5`)
- Das Ziel ist mit einem Schloss gesperrt, bis die Mission erfüllt ist
- Punkte gibt es für Münzen, Gegner, Missionsobjekte, Zeitbonus und übrige Leben
- Die Bestleistung wird im Browser gespeichert (`localStorage`)

### 9. Handy-Unterstützung
Auf Touch-Geräten erscheinen automatisch Steuerungstasten. Das Canvas skaliert sich an jede Bildschirmgrösse.

### 10. Testen
Ich habe das Spiel automatisiert mit **Playwright** (Headless-Browser) getestet: alle drei Levels laden, Steuerung reagiert, jedes Level kann abgeschlossen werden und es treten keine JavaScript-Fehler auf. Danach folgte das Feintuning beim Selberspielen.

---

## Projektstruktur

```
kai-stadtmissionen/
├── index.html      # Das komplette Spiel (HTML, CSS, JavaScript)
└── README.md       # Diese Dokumentation
```

## Lokal starten

Repository herunterladen und `index.html` im Browser öffnen – fertig. Kein Server, keine Installation nötig.

```bash
git clone https://github.com/dalipivaldrin/kai-stadtmissionen.git
cd kai-stadtmissionen
# index.html im Browser öffnen
```

## Ideen für später
- Weitere Missionen und Levels
- Bewegliche Plattformen und Power-ups
- Highscore-Liste
- Level-Editor

---

Entwickelt von **Valdrin Dalipi** – mit Unterstützung von KI (Claude) bei Planung und Programmierung.
