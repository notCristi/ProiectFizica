# Physik-Sandbox 2D — Interaktiver Mechanik-Simulator

Ein interaktiver 2D-Simulator für die klassische Mechanik, der im Webbrowser läuft.
Man kann Körper (Würfel, Kugeln, Rampen) auf eine Bühne setzen, sie mit Seilen, Federn
und Flaschenzügen verbinden, physikalische Parameter einstellen und die Simulation in
Echtzeit beobachten. Während der Simulation werden alle wirkenden **Kräfte als Pfeile**
dargestellt und Messgrößen (Geschwindigkeit, Beschleunigung, Energie …) als **Diagramme**
aufgezeichnet, die sich nach Excel exportieren lassen.

Das Projekt ist ein Physikprojekt für die Oberstufe und besteht aus zwei Teilen:

| Datei | Inhalt |
|-------|--------|
| `index.html` | Der eigentliche Simulator |
| `referat.html` | Das schriftliche Referat über **Reibung und Strömungswiderstand** |

> **Hinweis für den Einstieg:** Es ist keine Installation nötig. Es genügt, die fertige
> Datei `dist/index.html` in einem Browser zu öffnen (Doppelklick). Eine Online-Version
> liegt unter `https://notcristi.github.io/ProiectFizica/`.

---

## Was kann der Simulator?

- **Objekte hinzufügen:** Würfel, Quader, Kugeln, Dreiecke und Rampen aus einem Katalog
  von Vorlagen (Eis, Gummi, Stahlkugel, Bowlingkugel … jeweils mit realistischer Masse
  und Reibung).
- **Eigenschaften einstellen:** Masse, Reibungskoeffizient µ, Anfangsgeschwindigkeit,
  eine angreifende Kraft (mit Winkel), Elastizität (Stoßzahl), Skalierung, Rampenwinkel.
- **Verbindungen:** Seil (undehnbar), Feder (Hookesches Gesetz), Flaschenzug.
- **Globale Einstellungen:** Schwerkraft *g*, Luftwiderstand *Cₐ*, Simulationsdauer und
  Abspielgeschwindigkeit (0,1× bis 10×).
- **Fertige Experimente** per Klick (siehe unten).
- **Zwei Modi:** *Einfach* (nur Masse + Schwerkraft, ideal für den ersten Kontakt) und
  *Erweitert* (alle Steuerungen, Diagramme, Datenexport).
- **Mehrsprachig:** Deutsch, Englisch, Rumänisch (umschaltbar oben rechts; Standard Deutsch).

---

## Die zugrunde liegende Physik

Der Simulator rechnet in **echten SI-Einheiten** (Meter, Kilogramm, Sekunde, Newton).
Auf dem Bildschirm entspricht der Maßstab **80 Pixel = 1 Meter**. Die Bühne hat einen
unendlich breiten Boden auf Höhe 0 m.

Folgende physikalischen Modelle sind umgesetzt:

| Größe | Formel | Bemerkung |
|-------|--------|-----------|
| Gewichtskraft | **G = m · g** | *g* einstellbar (Standard 9,81 m/s²) |
| Normalkraft | **N** | aus den echten Kontaktkräften gemessen, nicht nur *m·g* angenommen |
| Reibung (Coulomb) | **F_R = µ · N** | wirkt entgegen der Bewegung, auch auf schiefen Ebenen |
| Luftwiderstand | **F_W = ½ · ρ · Cₐ · A · v²** | ρ = Luftdichte (1,225 kg/m³), A = Stirnfläche |
| Federkraft (Hooke) | **F = k · (L − L₀)** | mit leichter Materialdämpfung (ζ = 2 %) |
| Seil | undehnbare Längenbeschränkung | zieht nur, drückt nie (wird schlaff korrekt behandelt) |
| Flaschenzug | ideales Seil über einen Drehpunkt | z. B. für die Atwoodsche Fallmaschine |
| Trägheitsmoment | Scheibe ½mr², Quader m/12·(b²+h²) | bestimmt das Drehverhalten |

Besonderheiten, die den Simulator realistisch machen:

- **Gemessene statt angenommene Kräfte:** Normal- und Reibungskraft werden aus den
  tatsächlichen Kontakt-Impulsen der Physik-Engine berechnet. Steht z. B. ein Körper auf
  einem anderen, trägt die Bodenfläche das gesamte Gewicht darüber — wie in der Realität.
- **Lagerreibung an Seil/Flaschenzug:** Ein reales Pendel kommt nicht durch Luftwiderstand,
  sondern durch die Reibung an der Aufhängung zur Ruhe. Diese kleine, konstante Reibkraft
  (µ ≈ 0,05 am Haken, 0,03 an der Rolle) ist nachgebildet, damit Pendel in endlicher Zeit
  stoppen. Pro Verbindung kann sie als *ideal* (reibungsfrei) abgeschaltet werden.
- **Endgeschwindigkeit:** Beim freien Fall mit Luftwiderstand stellt sich automatisch die
  korrekte Grenzgeschwindigkeit ein (Fallschirm-Experiment).

### Genauigkeit der Rechnung

Pro Bild (Frame) wird die Simulation in **32 kleine Teilschritte** zerlegt und jeder
Teilschritt mehrfach iteriert. Das hält Stöße, Seile und Flaschenzüge auch bei großen
Kräften stabil. Eine eingebaute Kontrolle (die **Atwoodsche Fallmaschine**) vergleicht beim
Start des Debug-Modus das Simulationsergebnis mit der theoretischen Formel
*a = (m₁ − m₂)·g / (m₁ + m₂)* — die Abweichung liegt unter 2,5 %.

---

## Mitgelieferte Experimente

| Experiment | Behandeltes Thema |
|------------|-------------------|
| Block auf reibungsbehafteter Fläche | Gleitreibung, Abbremsen |
| Freier Fall — verschiedene Körper | Fallgesetz, Unabhängigkeit von der Masse |
| Block auf der schiefen Ebene | Normalkraft, Hangabtrieb |
| Atwoodsche Fallmaschine | Seil, Flaschenzug, Seilspannung |
| Federschwinger | Hookesches Gesetz, Schwingung |
| „Zug mit Hindernis" | Trägheit, plötzliches Abbremsen |
| Fallschirmspringer | Luftwiderstand, Endgeschwindigkeit |

Schnelleinstellungen erlauben zudem das Umschalten der Umgebung: **Mond** (g = 1,62 m/s²),
**Jupiter** (g = 24,8 m/s²), **perfektes Eis** (µ = 0,01) und **Vakuum** (kein Luftwiderstand).

---

## Diagramme und Datenexport

Für das ausgewählte Objekt können über **30 Größen** als Diagramm über die Zeit (oder
gegeneinander) aufgezeichnet werden, u. a.:

- Ort, Höhe, zurückgelegte Strecke, Bahnkurve (y über x)
- Geschwindigkeit (x, y, Betrag, v²) und Beschleunigung
- Kräfte: Gewicht, Normalkraft, Reibung, Luftwiderstand, angreifende Kraft, resultierende Kraft
- Energie: kinetisch, potentiell, mechanisch gesamt, Rotationsenergie
- Impuls und Leistung
- Zusammenhänge wie **F_R = µ·N** oder Luftwiderstand über v²

Die Daten lassen sich als **Excel-Datei (.xlsx)** exportieren — sauber in Spalten mit
Einheiten, sodass im Tabellenprogramm sofort eigene Diagramme erstellt werden können —
oder als **PNG-Bild** zum Einfügen in Word/PowerPoint.

---

## Hilfe durch künstliche Intelligenz (optional)

Über die Karte **„Mit KI erzeugen"** kann man eine Aufgabenstellung in normaler Sprache
eingeben (z. B. *„Ein 5-kg-Block auf einer 30°-Rampe mit kleiner Reibung"*). Eine KI baut
daraus automatisch die passende Szene auf. **Wichtig:** Die KI *löst die Aufgabe nicht* —
sie stellt nur den Versuchsaufbau bereit, den man dann selbst simuliert und auswertet.

Aus Sicherheitsgründen läuft diese Funktion über einen kleinen Vermittlungs-Server
(damit der Zugangsschlüssel zur KI nicht öffentlich sichtbar ist). Sie funktioniert nur in
der Online-Version; alle anderen Funktionen laufen vollständig offline.

---

## Eingesetzte Technik (kurz und allgemeinverständlich)

Das Projekt verwendet ausschließlich freie Webtechnologien und läuft in jedem modernen
Browser — ohne Installation, ohne Plugins.

| Werkzeug | Wofür |
|----------|-------|
| **TypeScript** | die Programmiersprache, in der der Simulator geschrieben ist (eine geprüfte Variante von JavaScript) |
| **Planck.js** | die Physik-Engine — eine bewährte Bibliothek (Box2D), die auch in vielen Computerspielen Stöße und Kräfte berechnet |
| **HTML5-Canvas** | die Zeichenfläche, auf der die Bühne, Objekte und Kraftpfeile gemalt werden |
| **Vite** | das Bau-Werkzeug, das den Quelltext in eine einzige, eigenständige Datei (`dist/index.html`) zusammenfasst |
| **SheetJS** | erzeugt die Excel-Exportdateien |

Der gesamte Simulator wird beim „Bauen" in **eine einzige HTML-Datei** verpackt. Diese
Datei enthält alles (Programm, Stil, Bilder) und kann ohne Internetverbindung geöffnet,
verschickt oder auf einem USB-Stick weitergegeben werden.

---

## Aufbau des Projekts (für Interessierte)

```
ProiectFizicaBun/
├─ index.html          Startseite des Simulators
├─ referat.html        Das schriftliche Referat
├─ dist/index.html     Fertige, eigenständige Version (zum Öffnen/Abgeben)
└─ src/                Der Quelltext, nach Aufgaben getrennt:
   ├─ physics/         Die Physikrechnung (Kräfte, Stöße, Seile, Federn, Flaschenzug)
   ├─ ui/              Bedienfeld, Datenexport, KI-Funktion
   ├─ render.ts        Das Zeichnen der Bühne und der Kraftpfeile
   ├─ graphs.ts        Die Diagramme
   ├─ camera.ts        Zoom und Verschieben der Ansicht
   └─ i18n.ts          Die Übersetzungen (DE/EN/RO)
```

Der Quelltext ist klar in Teilbereiche gegliedert: Die reine **Physikrechnung** ist von
der **Darstellung** und der **Bedienung** getrennt. So bleibt z. B. die Berechnung der
Reibungskraft an einer einzigen, gut dokumentierten Stelle.

---

## Selbst starten (nur bei Bedarf)

Für das reine Benutzen genügt das Öffnen von `dist/index.html`. Wer den Quelltext
verändern möchte, braucht [Node.js](https://nodejs.org) und gibt im Projektordner ein:

```bash
npm install      # einmalig: benötigte Bibliotheken laden
npm run dev      # Entwicklungsversion starten (öffnet eine lokale Adresse)
npm run build    # fertige Datei dist/index.html erzeugen
```

---

## Bedienung in Kürze

- **Leertaste** — Start/Pause der Simulation
- **Objekt anklicken** — auswählen; dann erscheinen Anfasser zum Drehen und Skalieren
- **Rechte Maustaste + ziehen** — Ansicht verschieben; **Mausrad** — zoomen
- **Entf** — ausgewähltes Objekt löschen
- **G** — Raster-Einrasten an-/ausschalten

---

*Physikprojekt — Mechanik-Simulator · Autor: Cristian.*
