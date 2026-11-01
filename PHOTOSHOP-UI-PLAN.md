# Plan: GIMP-Oberfläche und Bedienung an Photoshop angleichen

Stand: 2026-11-01 · Basis: GIMP 3.3.1 (master-Fork, GTK3)

## 1. Ziel und Grundsätze

Ziel ist ein GIMP-Fork, der sich für Photoshop-Nutzer vertraut anfühlt:
Werkzeuganordnung, Tastenkürzel, Menüstruktur, Panel-Layout, Optionsleiste
und Optik so nah an Photoshop wie technisch möglich.

Grundsätze:

- **Schichtweise vorgehen:** erst Konfiguration (risikoarm, schnell), dann
  Theme/CSS, dann Menüs, zuletzt echte C-Änderungen an Widgets.
- **Upstream-fähig bleiben:** Änderungen möglichst als eigene Defaults/Dateien
  statt Umbauten im Kern, damit spätere Merges von GNOME/gimp nicht weh tun.
- **Rechtlich sauber:** Kein Adobe-Logo, keine Adobe-Icons, nicht den Namen
  „Photoshop“ im Produkt verwenden. Layout, Bedienkonzepte und Kürzel
  nachbilden ist ok, Icons werden selbst gezeichnet (ähnlicher Stil, eigenes
  Design).

## 2. Recherche: vorhandene Bibliotheken, Add-ons und Vorarbeiten

| Projekt | Was es liefert | GIMP 3? | Lizenz | Nutzen für uns |
|---|---|---|---|---|
| **PhotoGIMP** (Diolinux) | `shortcutsrc`, `toolrc`, `sessionrc`, `dockrc`, `gimprc`, `contextrc`, `templaterc`, `theme.css`, Splash | ja (nur 3.x) | GPL-3.0 | **Hauptquelle.** Kürzel und Werkzeugreihenfolge übernehmen wir als neue Defaults in `etc/`. |
| `etc/ps-menurc` (früher im GIMP-Baum) | Photoshop-CS4-Kürzel | nein (Format `menurc` aus 2.x) | GPL | Nur als Referenz; GIMP 3 liest `shortcutsrc` und ignoriert `menurc`. |
| **GimpPs** | Kürzel, Icon-Theme, Layout | nein (2.8/2.10) | GPL | Ideen für Icons/Layout, nicht direkt nutzbar. |
| **HyperflatGraphite** | Flaches dunkles Theme für GIMP 3 | ja | prüfen | Vorlage für das CSS-Theme (Photoshop-artiges Grau). |
| **gimp-cc-themes** (draekko) | „CC“-artige Themes | nein (GTK2) | prüfen | Nur optische Referenz für Farbwerte. |

Fazit: Es gibt **keine Bibliothek**, die die Oberfläche umbaut. PhotoGIMP
ist das brauchbarste Add-on, aber es tauscht nur Konfigurationsdateien.
Alles, was darüber hinausgeht (Optionsleiste, Panel-Verhalten, Ebenen-Panel),
muss im C-Code dieses Forks passieren.

## 3. Was im Code wo sitzt

| Bereich | Ort |
|---|---|
| Standard-Konfiguration (Werkzeuge, Fenster, Docks) | `etc/toolrc`, `etc/sessionrc`, `etc/gimprc.in`, `etc/templaterc` |
| Tastenkürzel (Defaults) | `app/actions/*-actions.c` (z. B. `edit-actions.c:97`), Werkzeugkürzel in `app/tools/*` `register`-Funktionen |
| Menüs | `menus/image-menu.ui.in.in`, `menus/*-menu.ui` |
| Themes (GTK3-CSS) | `themes/Default/*.css`, `etc/gimp.css` |
| Icons, Splash | Submodul `gimp-data` |
| Werkzeugkasten | `app/widgets/gimptoolbox.c`, `gimptoolpalette.c` |
| Werkzeugoptionen | `app/widgets/gimptooloptionseditor.c`, `app/tools/*-options.c` |
| Bildfenster, Statusleiste | `app/display/gimpimagewindow.c`, `gimpstatusbar.c` |
| Einzelfenster-Modus | `app/config/gimpguiconfig.c` (`single-window-mode`) |

## 4. Phasenplan

### Phase 1 – Defaults und Konfiguration (geringes Risiko) ✅ umgesetzt

> Details und Kürzel-Tabelle: [`docs/GIMPSHOP-SHORTCUTS.md`](docs/GIMPSHOP-SHORTCUTS.md).
> Eigener Konfigurationsordner `GIMPshop` (statt `GIMP`), damit die neuen
> Defaults auch bei vorhandener GIMP-Installation greifen. Damit entfällt
> die unten genannte `shortcutsrc`-Migration.
> Getestet in einem Linux-Build unter Xvfb: alle Werkzeugkürzel, Strg+N/J/E/L/M/U/B/T,
> Strg+Alt+I/C, Strg+K, Strg+0/1, F5–F8, Umschalt+F6, Alt/Strg+Rücktaste.
> Technische Änderungen: bis zu 5 Standard-Kürzel pro Aktion
> (`GIMP_ACTION_MAX_ACCELS`), Funktionstasten haben Vorrang vor dem Fokus-Widget.

1. **Einzelfenster-Modus als Standard** in `gimpguiconfig.c` / `gimprc.in`.
2. **Werkzeugreihenfolge und -gruppen** in `etc/toolrc` wie in Photoshop:
   Verschieben → Auswahlrechteck/Ellipse → Lasso-Gruppe → Zauberstab/Schnellauswahl
   → Freistellen → Pipette → Reparatur/Kopierstempel → Pinsel/Stift → Radierer
   → Verlauf/Füllen → Weichzeichner/Schärfen/Wischfinger → Abwedler/Nachbelichter
   → Pfade → Text → Hand → Zoom.
3. **Panel-Layout** in `etc/sessionrc`: Werkzeugleiste schmal links (einspaltig),
   rechts Dock mit oben „Farbe/Farbfelder“, Mitte „Eigenschaften/Korrekturen“,
   unten „Ebenen/Kanäle/Pfade“; Protokoll (Undo) als Tab.
4. **Tastenkürzel** aus PhotoGIMP übernehmen, aber als Code-Defaults in
   `app/actions/*.c` und Werkzeug-Registrierungen eintragen (nicht nur als
   Nutzerdatei), z. B. `Strg+T` frei transformieren, `Strg+J` Ebene duplizieren,
   `Strg+E` auf eine Ebene reduzieren, `Strg+D` Auswahl aufheben, `Strg+Shift+I`
   Auswahl umkehren, `Strg+L` Tonwertkorrektur, `Strg+M` Kurven, `Strg+U`
   Farbton/Sättigung, `Strg+Alt+I` Bildgröße, `[`/`]` Pinselgröße,
   `Strg+0`/`Strg+1` Zoom, Leertaste = Hand, `Alt` = Pipette im Malwerkzeug.
5. **Vorlagen** (`etc/templaterc`) an die Photoshop-Standardgrößen anpassen.
6. Zusätzlich die Datei `shortcutsrc` für Nutzer mit bestehendem Profil
   ausliefern und beim ersten Start migrieren.

### Phase 2 – Optik und Branding ✅ größtenteils umgesetzt

Umgesetzt:

1. **Theme „GIMPshop“** (`themes/GIMPshop/`) als neues Standard-Theme, in drei
   Helligkeitsstufen wie in Photoshop: Dunkel (`#323232`, Standard),
   Mittelgrau (`#535353`) und Hell (`#F0F0F0`). Die Stufe stellt man unter
   *Einstellungen → Oberfläche → Theme → Farbschema* ein.
   `gimpshop.css` ergänzt Photoshop-typische Details: blaue Hervorhebung in
   Menüs, blaue Checkboxen, Radio-Buttons und Textauswahl, blauer
   Standard-Button in Dialogen, flache Panel-Reiter mit blauer Unterstreichung.
   GIMPs bisheriges Theme bleibt als „GIMP Classic“ auswählbar.
2. **Name „GIMPshop“** in Fenstertitel, Programmname (`--version`),
   Info-Dialog („Based on GIMP …“), Willkommensdialog, Desktop-Datei und
   Windows-Dateieigenschaften der `.exe`.
3. **Eigenes Logo, Programm-Icon und Splash-Screen** (`data/gimpshop/`):
   drei versetzte „Ebenen-Karten“ mit Pinselstrich, bewusst *kein*
   Adobe-ähnliches Quadrat mit Buchstaben. SVG-Quellen liegen bei,
   PNG/ICO sind daraus gerendert. Das Windows-`.exe`-Icon nutzt `gimpshop.ico`.
4. **Ruhige Arbeitsfläche:** keine Wilber-Silhouette mehr im leeren
   Bildfenster und oben in der Werkzeugleiste, stattdessen ein dezenter
   Starthinweis (nur in stabilen Builds, Entwicklerversionen zeigen GIMPs
   Hinweis „Unstable Development Version“).

Offen:

- Eigenes Werkzeug-Icon-Set. Bis dahin nutzt GIMPshop GIMPs einfarbige
  „Symbolic“-Icons, die Photoshops grauen Icons schon recht nahe kommen.
- Programmdatei heißt weiterhin `gimp-3.3.exe`, auch Installer-Name und
  Linux-Icon (`org.gimp.GIMP`) sind noch unverändert. Ein Umbenennen
  betrifft viele Build- und Installer-Skripte und kommt gesammelt mit dem
  Installer.
- Neue Texte (Willkommen, Starthinweis, „Based on GIMP …“) sind noch nicht
  übersetzt und erscheinen auf Englisch.

### Phase 3 – Menüs und Begriffe ✅ größtenteils umgesetzt

Umgesetzt:

1. **Menüleiste in Photoshop-Reihenfolge:** Datei · Bearbeiten · Bild · Ebene ·
   Schrift · Auswahl · Filter · Ansicht · Werkzeuge · Fenster · Hilfe.
2. **Bild → Korrekturen** enthält alles aus dem früheren Menü „Farben“
   (Helligkeit/Kontrast, Tonwerte, Kurven, Farbton/Sättigung, Farbbalance,
   Belichtung, Umkehren, Schwellwert, …). Plug-ins und GEGL-Filter, die sich
   unter `<Image>/Colors` registrieren, werden automatisch nach
   `Bild → Korrekturen` umgeleitet (`gimp_ui_manager_remap_path()` in
   `app/widgets/gimpuimanager.c`), es entsteht kein zweites „Farben“-Menü.
3. **Bild:** Auto-Funktionen (Weißabgleich, Kontrastspreizung,
   Farbverbesserung) direkt im Menü, *Bildgröße* und *Arbeitsfläche* oben im
   Größen-Abschnitt, *Duplizieren* weiter unten wie in Photoshop.
4. **Neues Menü „Schrift“:** Textwerkzeug, Schriften, Text in Pfad,
   Text entlang Pfad, Rastern.
5. **Auswahl → Auswahl verändern** (Rand, Erweitern, Verkleinern, Weiche
   Kante) als Untermenü. Die Plug-in-Ankerpunkte `[Modify]` und `Modify`
   funktionieren weiter.
6. **Fenster** listet alle Bedienfelder direkt (kein Untermenü
   „Andockbare Dialoge“ mehr).
7. **Begriffe wie in Photoshop** (englisch und deutsch): Bildgröße,
   Auswahl aufheben, Auswahl umkehren, Erweitern, Verkleinern, Farbbereich,
   Weiche Kante, Korrekturen, Schrift. Deutsche Übersetzungen dafür in
   `po/de.po`.

Offen:

- „Ebene“ weiter angleichen (Ebenenstil, Schnittmaske, Smart-Objekte hängen
  von Phase 4 bzw. neuen Funktionen ab).
- Neue englische Begriffe sind nur ins Deutsche übersetzt; andere Sprachen
  zeigen für diese Einträge Englisch.

### Phase 4 – Bedienung im C-Code (größter Aufwand, größter Effekt)

1. **Optionsleiste oben** (wichtigstes Photoshop-Merkmal): die Werkzeugoptionen
   als horizontale Leiste unter der Menüleiste im Bildfenster
   (`gimpimagewindow.c`). Dafür eine horizontale Darstellung der Options-GUIs
   in `app/tools/*-options.c` bauen (kompakte Widgets: Größe, Härte,
   Deckkraft, Fluss, Modus). Das klassische Optionen-Dock bleibt optional.
2. **Werkzeugkasten:** einspaltig, Gruppen per Rechtsklick/Langklick ausklappen,
   Vorder-/Hintergrundfarbe unten, Maskierungsmodus-Knopf (Quick Mask) darunter.
3. **Ebenen-Panel:** Mischmodus-Dropdown + Deckkraft + Fläche oben,
   Sperr-Knöpfe, unten Knopfleiste (fx/Maske/Korrekturebene/Gruppe/Neu/Löschen),
   Alt-Klick auf Auge = andere ausblenden.
4. **Verhalten:**
   - Verschieben-Werkzeug mit „automatisch auswählen“ und Transformationsrahmen.
   - Strg+T: „frei transformieren“ als ein Werkzeug (Unified Transform) mit
     Enter zum Bestätigen, Esc zum Abbrechen.
   - Ebenen standardmäßig auf Bildgröße, Freistellen löscht Pixel optional.
   - Neue Textebene per Klick, Bestätigen mit Strg+Enter.
5. **Statusleiste** unten links mit Zoomfaktor und Dokumentinfo.
6. **Korrekturebenen**: GIMP 3 hat nicht-destruktive Filter (NDE). Diese im
   Ebenen-Panel als „Korrekturebene“-Knopf zugänglich machen.

### Phase 5 – Qualität und Verteilung

- Build mit Meson in CI (Linux), Smoke-Test, Screenshots pro Phase.
- Umschalter in den Einstellungen: „Photoshop-artiges Layout / klassisches GIMP“,
  damit Altnutzer nicht ausgesperrt werden.
- Regelmäßig `upstream/master` mergen; Änderungen möglichst in eigenen Dateien halten.

## 5. Reihenfolge und grober Aufwand

| Phase | Aufwand | Wirkung |
|---|---|---|
| 1 Defaults/Kürzel | klein (Tage) | hoch |
| 2 Theme/Icons | mittel (Icons kosten Zeit) | hoch |
| 3 Menüs/Begriffe | mittel | mittel–hoch |
| 4 Optionsleiste, Ebenen-Panel, Verhalten | groß (Wochen) | sehr hoch |
| 5 CI/Umschalter | klein–mittel | Pflege |

Vorschlag: mit Phase 1 starten (PhotoGIMP-Werte als Code-Defaults), dann
Phase 2 (Theme), danach die Optionsleiste aus Phase 4 vorziehen, weil sie den
größten Wiedererkennungswert hat.

## 6. Offene Fragen

- Soll das klassische GIMP-Layout per Schalter erhalten bleiben?
- ~~Ziel-Plattformen~~ → **Windows** (GitHub-Action `.github/workflows/windows-build.yml`).
- ~~Produktname~~ → **GIMPshop**. Fenstertitel, Dialoge, Icon und Splash sind
  umgestellt (Phase 2). Offen: Name der `.exe` und des Installers.

## Quellen

- PhotoGIMP: https://github.com/Diolinux/PhotoGIMP
- HyperflatGraphite (GIMP-3-Theme): https://piefed.jeena.net/post/347986
- gimp-cc-themes: https://github.com/draekko/gimp-cc-themes
- `menurc` → `shortcutsrc` in GIMP 3: https://www.gimp-forum.net/printthread.php?tid=10872
- GimpPs / Photoshop-Kürzel für 2.10: https://www.linuxuprising.com/2018/11/configure-gimp-210-to-use-photoshop.html
- Theme/Icon-Einstellungen in GIMP 3: https://www.cloudspress.com/how-to-make-gimp-feel-more-like-photoshop/
