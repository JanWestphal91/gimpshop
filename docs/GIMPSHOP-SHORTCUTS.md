# GIMPshop – Tastenkürzel und Standard-Layout (Phase 1)

GIMPshop übernimmt die Windows-Kürzel von Photoshop als **Standardwerte**.
Alles lässt sich weiterhin unter *Bearbeiten → Tastenkürzel* (Strg+Alt+Umschalt+K)
anpassen. Auf macOS wird `Strg` automatisch zu `Cmd`.

GIMPshop speichert seine Einstellungen in einem eigenen Ordner
(`%APPDATA%\GIMPshop\3.3` unter Windows, `~/.config/GIMPshop/3.3` unter Linux).
Eine vorhandene GIMP-Installation bleibt also unberührt, und die neuen
Standardwerte gelten auch dann, wenn GIMP schon einmal benutzt wurde.

## Werkzeuge

| Taste | Werkzeug | Taste | Werkzeug |
|---|---|---|---|
| V | Verschieben | I | Pipette (Farbpipette) |
| M | Rechteckauswahl | Umschalt+I | Maßband |
| Umschalt+M | Ellipsenauswahl | J | Heilen (Reparatur-Pinsel) |
| L | Lasso (Freie Auswahl) | B | Pinsel |
| Umschalt+L | Magnetisches Lasso (Schere) | N | Bleistift |
| W | Zauberstab | S | Kopierstempel (Klonen) |
| Umschalt+W | Nach Farbe auswählen | E | Radiergummi |
| C | Freistellen | G | Verlauf |
| Strg+T | Frei transformieren (Vereinheitlichte Transformation) | Umschalt+G | Füllen (Farbeimer) |
| O | Abwedler/Nachbelichter | P | Zeichenstift (Pfade) |
| T | Text | U | Form |
| Z | Zoom | Leertaste (halten) | Hand / Ansicht verschieben |
| D | Standardfarben | X | Farben tauschen |
| Q | Maskierungsmodus (Schnellmaske) | `[` / `]` | Pinselgröße − / + |
| Tab | Bedienfelder aus-/einblenden | | |

## Bearbeiten und Auswahl

| Kürzel | Aktion |
|---|---|
| Strg+Z / Strg+Umschalt+Z | Rückgängig / Wiederholen (Strg+Y geht weiterhin) |
| Strg+X / Strg+C / Strg+V | Ausschneiden / Kopieren / Einfügen |
| Strg+Umschalt+C | Auf eine Ebene reduziert kopieren |
| Strg+Umschalt+V | An Originalposition einfügen |
| Entf / Rücktaste | Auswahl löschen |
| Alt+Rücktaste | Mit Vordergrundfarbe füllen |
| Strg+Rücktaste | Mit Hintergrundfarbe füllen |
| Strg+A | Alles auswählen |
| Strg+D | Auswahl aufheben |
| Strg+Umschalt+I | Auswahl umkehren |
| Umschalt+F6 | Weiche Auswahlkante |
| Strg+K | Voreinstellungen |

## Bild und Korrekturen

| Kürzel | Aktion |
|---|---|
| Strg+N / Strg+O / Strg+S / Strg+Umschalt+S | Neu / Öffnen / Speichern / Speichern unter |
| Strg+Alt+Umschalt+W | Exportieren als |
| F12 | Zurück zur letzten Version |
| Strg+W / Strg+Alt+W | Schließen / Alle schließen |
| Strg+Alt+I | Bildgröße |
| Strg+Alt+C | Arbeitsfläche (Leinwandgröße) |
| Strg+L | Tonwertkorrektur |
| Strg+M | Gradationskurven |
| Strg+U | Farbton/Sättigung |
| Strg+B | Farbbalance |
| Strg+Umschalt+U | Sättigung verringern (Entsättigen) |
| Strg+I | Umkehren (Negativ) |
| Strg+Alt+F | Letzten Filter wiederholen |

## Ebenen

| Kürzel | Aktion |
|---|---|
| Strg+Umschalt+N | Neue Ebene |
| Strg+J | Ebene duplizieren |
| Strg+G | Neue Ebenengruppe |
| Strg+E | Auf eine Ebene reduzieren (nach unten zusammenfügen) |
| Strg+Umschalt+E | Sichtbare auf eine Ebene reduzieren |
| Strg+Alt+Umschalt+E | Neue Ebene aus Sichtbarem (Stempeln) |
| Strg+] / Strg+[ | Ebene nach oben / unten |
| Strg+Umschalt+] / Strg+Umschalt+[ | Ganz nach oben / ganz nach unten |
| Alt+] / Alt+[ | Nächste Ebene darüber / darunter auswählen |
| Strg+Alt+H | Schwebende Auswahl verankern (GIMP-spezifisch) |

## Ansicht und Bedienfelder

| Kürzel | Aktion |
|---|---|
| Strg++ / Strg+- | Einzoomen / Auszoomen |
| Strg+0 | Ganzes Bild anzeigen |
| Strg+1 | 100 % |
| Strg+R | Lineale |
| Strg+H | Auswahlkante ein-/ausblenden (Extras) |
| Strg+Ö (`;`) | Hilfslinien ein-/ausblenden |
| Strg+Umschalt+Ö (`;`) | An Hilfslinien ausrichten |
| Strg+Ä (`'`) | Raster ein-/ausblenden |
| F11 | Vollbild |
| F5 / F6 / F7 / F8 | Pinsel / Farbe / Ebenen / Info |
| Strg+F oder `/` | Aktion suchen (wie die Photoshop-Suche) |
| Strg+2 … Strg+9 | Zuletzt geöffnete Datei 2–9 (Strg+1/Strg+0 sind fürs Zoomen reserviert) |

Funktionstasten (F1–F12) haben in GIMPshop immer Vorrang vor dem fokussierten
Bedienelement. Ohne das würden GTK-Fensterteiler F6, F8 und Umschalt+F6
abfangen.

> Hinweis zu deutschen Tastaturen: Bei Kürzeln mit `[`, `]`, `;` und `'`
> zählt die Taste, auf der das Zeichen bei US-Belegung liegt. Unter Windows
> mit deutscher Belegung entspricht `;` der Taste Ö und `'` der Taste Ä.
> `[`/`]` brauchen hier AltGr. Eigene Zuordnungen sind im Tastenkürzel-Editor
> jederzeit möglich.

## Standard-Layout

- **Einzelfenster-Modus**, Fenster startet maximiert.
- **Links:** schmale Werkzeugleiste in Photoshop-Reihenfolge
  (Verschieben → Auswahlrechteck → Lasso → Zauberstab → Freistellen →
  Transformieren → Verformen → Pipette → Heilen → Pinsel → Stempel →
  Radiergummi → Verlauf → Weichzeichner → Abwedler → Zeichenstift → Text →
  Form → Zoom).
- **Rechts:** oben *Farbe / Farbfelder*, Mitte *Werkzeugoptionen / Pinsel /
  Protokoll / Histogramm* (bis Phase 4 die Optionsleiste oben bringt), unten
  *Ebenen / Kanäle / Pfade*.
- **Neue Vorlagen** mit weißem Hintergrund: GIMPshop-Standard (2100×1500,
  300 ppi), Fotoformate, Social-Media-Formate, Web und YouTube.

## Bekannte Punkte

- Beim allerersten Start kann das Freistellen-Werkzeug aktiv sein statt
  Verschieben. Das ist bestehendes GIMP-Verhalten (die erste Werkzeugvorgabe
  wird übernommen). Ab dem zweiten Start gilt das zuletzt benutzte Werkzeug.
- Werkzeuge ohne Photoshop-Gegenstück haben kein Kürzel mehr
  (Ausrichten, Verkrümmen, Käfig, Griff- und 3D-Transformation, Verschmieren).
  Sie bleiben über die Werkzeugleiste und das Menü *Werkzeuge* erreichbar.

## Unterschiede zu GIMP (Auswahl)

| Kürzel | GIMP | GIMPshop |
|---|---|---|
| Strg+D | Bild duplizieren | Auswahl aufheben |
| Strg+E | Exportieren | Nach unten zusammenfügen |
| Strg+J | Fenster anpassen | Ebene duplizieren |
| Strg+L | Ebenen-Dialog | Tonwertkorrektur |
| Strg+M | Sichtbare zusammenfügen | Gradationskurven |
| Strg+T | Auswahl anzeigen | Frei transformieren |
| Strg+H | Verankern | Auswahl anzeigen |
| Strg+I | Auswahl umkehren | Bild umkehren (Negativ) |
| Strg+B | Werkzeugkasten | Farbbalance |
| Strg+G | Verläufe-Dialog | Neue Ebenengruppe |
| Strg+F | Filter wiederholen | Aktion suchen |
| Strg+Umschalt+V | Als neues Bild einfügen | An Originalposition einfügen |
