## Changelog

### next

* Geplante weitere Verbesserungen
* Weitere Optimierungen für Bedienbarkeit, Dokumentation und Add-on-Kompatibilität

### 1.6.1.11

#### UX / Adaptive Layout

* Adaptive UX Engine für Mobile, Tablet und Desktop ergänzt.
* Neue Darstellungsmodi in der Oberfläche:
  * Auto
  * Mobil
  * Tablet
  * Desktop
* Auto-Modus erkennt Viewport, Touch-Fähigkeit, Pointer-Typ und Ausrichtung und wählt daraus das passende UX-Profil.
* Die gewählte Darstellung wird lokal im Browser gespeichert.
* Der bisherige Desktop-Modus bleibt erhalten, ist jetzt aber Teil des neuen Desktop-Profils.
* Der Button „Desktop-Modus“ wechselt künftig sauber zwischen Desktop-Workbench und automatischer klassischer Ansicht.

#### Mobile UX

* Eigene Mobile-Ansicht für iPhone und Android vorbereitet.
* Header, Navigation, Karten, Aktionen und Formulare wurden für kleine Touch-Displays angepasst.
* Navigation wird auf Mobile als kompakte horizontale Touch-Navigation dargestellt.
* Eingeklappte Sidebar kann auf Mobile nicht mehr als defekte vertikale Icon-Leiste hängen bleiben.
* Live Console wird auf Mobile als kompakter Bottom-Sheet-Bereich behandelt.
* iOS Safe-Area wird berücksichtigt, damit Inhalte und Bedienelemente nicht hinter Browser- oder Systemleisten verschwinden.
* Hauptinhalt bleibt oberhalb der Live Console scrollbar.
* Konsolenbedienung, Eingabefeld und Senden-Button bleiben erreichbar.

#### Tablet UX

* Eigene Tablet-Zwischenansicht für iPad, Android-Tablets und Touch-Laptops ergänzt.
* Tablet-Navigation ist touchfreundlich und horizontal scrollbar.
* Dashboard und Karten nutzen den verfügbaren Platz besser als Mobile, bleiben aber einfacher als die Desktop-Workbench.
* Dateimanager nutzt auf größeren Tablets weiterhin eine geteilte Explorer-/Editor-Ansicht.
* Live Console bleibt als stabiles Dock erreichbar, ohne den Inhalt unbrauchbar zu verdecken.

#### Dateimanager / Mobile

* Dateiliste wird auf Mobile als Kartenliste dargestellt statt als breite Tabelle.
* Dateiaktionen bleiben auf kleinen Displays erreichbar und umbrechen kontrolliert.
* Dateiname, Typ, Größe, Änderungsdatum und Aktionen bleiben auf Mobile lesbar.
* Editor und Vollbild-Editor berücksichtigen die mobile Live Console.

#### Maintenance

* Version auf `WinoMCConsole/1.6.1.11` angehoben.
* Lokale Syntaxprüfung mit `python3 -m py_compile` durchgeführt.
* JavaScript-Syntaxprüfung mit `node --check` durchgeführt.
* Bestehende CodeQL-/Security-Umbauten aus den vorherigen Versionen bleiben erhalten.

### 1.6.1.9

#### Web Console / Layout

* Live-Konsole im klassischen Layout robuster gegen ungünstige Browsergrößen gemacht.
* Bedienelemente der Live-Konsole bleiben jetzt erreichbar, auch wenn das Fenster in den mittelbreiten Layoutbereich wechselt.
* Console-Buttons laufen bei wenig Breite nicht mehr unterhalb des sichtbaren Dock-Bereichs aus dem Bild.
* Console-Buttons bleiben auf einer horizontal scrollbaren Bedienleiste, statt den Header unkontrolliert nach unten zu vergrößern.
* Eingabezeile und Logbereich werden innerhalb der festen Live-Konsole stabiler berechnet.
* Eingeklappte Konsole wurde leicht erhöht, damit Kopfzeile und Bedienung nicht abgeschnitten werden.
* Bei niedriger Fensterhöhe wird die Konsolenhöhe begrenzt, damit keine Bedienelemente aus dem sichtbaren Bereich rutschen.

#### Maintenance

* Version auf `WinoMCConsole/1.6.1.9` angehoben.
* CodeQL-/Pfadsicherheitslogik aus 1.6.1.7 und der Dateiexplorer-Scrollfix aus 1.6.1.8 bleiben erhalten.
* Änderung betrifft nur Layout/CSS der Live-Konsole.

### 1.6.1.8

#### Web Console / Dateimanager

* Scrollverhalten im klassischen Dateiexplorer der Webkonsole korrigiert.
* Die Dateiliste erhält im klassischen Modus nun einen eigenen vertikalen Scrollbereich.
* Dateien am unteren Ende langer Ordnerlisten bleiben dadurch erreichbar, auch wenn die Live-Konsole unten eingeblendet ist.
* Tabellenkopf im Dateimanager bleibt beim Scrollen sichtbar.
* Desktop-Modus bleibt unverändert, da dort bereits das Fenster selbst korrekt scrollt.

#### UI / Layout

* Klassischer Dateimanager berücksichtigt die feste Konsolenleiste am unteren Bildschirmrand besser.
* Explorer- und Editor-Karte nutzen im klassischen Modus nun eine begrenzte Höhe mit internem Scrollen statt unkontrolliert nach unten aus dem sichtbaren Bereich zu wachsen.
* Touch-/Ingress-Scrolling für Home Assistant verbessert.

#### Maintenance

* Version auf `WinoMCConsole/1.6.1.8` angehoben.
* Keine Änderungen an der CodeQL-Sicherheitslogik aus 1.6.1.7.
* Keine Änderungen an Nutzeroptionen oder API-Endpunkten.

### 1.6.1.7

#### Security / CodeQL

* Weitere Reduzierung der offenen CodeQL-Meldungen im `winomc-console-server`.
* Dateisystem-Zugriffe strukturell überarbeitet, damit geprüfte Pfade nicht mehr als rohe, request-beeinflusste Strings bis zu `open`, `listdir`, `stat`, `getsize`, `remove`, `replace`, `copy2`, `move` oder `mkstemp` weitergereicht werden.
* Neue interne `VerifiedPath`-Pfadklasse ergänzt:
  * Pfade werden weiterhin normalisiert und gegen erlaubte WinoMC-Root-Verzeichnisse geprüft.
  * Symlink-Komponenten bleiben blockiert.
  * Dateisystem-Sinks erhalten nun einen geprüften `PathLike`-Wrapper statt direkt den aus Request-Daten abgeleiteten String.
* Root-, interne und Runtime-Pfadoperationen vereinheitlicht über:
  * `verify_root_path(...)`
  * `verify_internal_path(...)`
  * `verify_runtime_path(...)`
* Mehrere verbleibende `py/path-injection`-Flows aus den allgemeinen Dateioperationen entschärft, insbesondere bei:
  * Datei öffnen
  * Existenzprüfung
  * Dateitypprüfung
  * Verzeichnislisting
  * Dateigröße und Stat-Informationen
  * Ordnererstellung
  * temporären Dateien
  * Entfernen, Ersetzen, Kopieren und Verschieben
* FIFO-Zugriff für Konsolenbefehle auf geprüften Runtime-Pfad umgestellt.
* Dateierstellung weiter gehärtet: der sichere Open-Opener verwendet nun restriktive Rechte `0600` statt `0666`.

#### Reliability

* Unbenutzte lokale Variablen entfernt:
  * `zip_path = None` im temporären ZIP-Zweig
  * `log_parent` beim Start der Webkonsole
* Temporäre ZIP-Erstellung, gespeicherter ZIP-Export, Upload, Download, Editor, Papierkorb und ZIP-Slip-Schutz erneut per Smoke-Test geprüft.
* Datei erneut mit `python3 -m py_compile` syntaktisch geprüft.

#### Maintenance

* Keine CodeQL-/LGTM-Suppression-Kommentare verwendet.
* Keine reine Alert-Unterdrückung.
* Zentrale Sicherheitslogik klarer getrennt in:
  * Pfadvalidierung
  * geprüfte `PathLike`-Objekte
  * eigentliche Dateisystemoperationen
* Version auf `WinoMCConsole/1.6.1.7` angehoben.

### 1.6.1.6

#### Security / CodeQL

* Download- und ZIP-Auslieferung im `winomc-console-server` strukturell überarbeitet.
* Die generische Funktion `_send_file(file_path, ...)` wurde entfernt, damit kein request-beeinflusster Pfad mehr als Parameter bis zu `open(...)` weitergereicht wird.
* Temporäre ZIP-Downloads werden nun über einen serverseitig erzeugten temporären Dateihandle erstellt und direkt aus diesem Handle gestreamt.
* ZIP-Dateinamen für temporäre Downloads und gespeicherte Exporte werden serverseitig erzeugt und nicht mehr aus Request-Werten wie `root` zusammengesetzt.
* Der von CodeQL gemeldete Flow aus `create_zip_from_paths(...)` über `zip_info["zip_path"]` in `_send_file(...)` wurde entfernt.
* Normale Datei-Downloads laufen über eine eigene Root-/Relativpfad-Downloadfunktion statt über einen generischen Vollpfad-Streamer.
* ZIP-Ziele für gespeicherte Exporte werden über `mkstemp_root(...)` im erlaubten Export-Root erzeugt.
* Temporäre ZIPs ohne Export-Speicherung verwenden `tempfile.TemporaryFile(...)` und besitzen keinen von außen beeinflussbaren Pfadnamen.

#### Reliability

* ZIP-Streams werden nach der Auslieferung kontrolliert geschlossen.
* Unvollständige gespeicherte ZIP-Dateien werden bei Fehlern bereinigt.
* ZIP-Suffix-Erzeugung korrigiert, sodass gespeicherte Exportdateien wieder sauber mit `.zip` enden.
* Datei weiterhin syntaktisch mit `python3 -m py_compile` geprüft.

#### Maintenance

* Keine CodeQL-/LGTM-Suppression-Kommentare verwendet.
* Keine reine Alert-Unterdrückung.
* Der problematische Pfad-Parameter-Flow wurde aus dem Code entfernt.
* Interne Download-Auslieferung klarer getrennt in:
  * `_send_stream(...)` für bereits geöffnete sichere Handles
  * `_send_root_download(...)` für normale Downloads aus erlaubten WinoMC-Roots
  * `_send_zip_info(...)` für temporäre ZIP-Streams

### 1.6.1.5

#### Security / CodeQL

* Weitere echte Behebung der CodeQL-Meldungen `py/path-injection` im `winomc-console-server`.
* Die zentrale Änderung: Dateioperationen führen die Pfadvalidierung jetzt direkt in derselben Funktion aus, in der anschließend der Dateisystemzugriff erfolgt.
* `listdir_root(...)` und weitere Root-Dateioperationen wurden so umgebaut, dass CodeQL den Ablauf besser nachvollziehen kann:
  * Pfad wird mit `os.path.abspath(...)`, `os.path.realpath(...)` und `os.path.normpath(...)` kanonisiert.
  * Der kanonisierte Pfad wird direkt gegen den erlaubten Root-Bereich geprüft.
  * Erst danach erfolgt `os.listdir(...)`, `os.stat(...)`, `os.path.getsize(...)`, `open(...)`, `os.makedirs(...)`, `os.replace(...)`, `os.remove(...)`, `shutil.move(...)` oder `shutil.rmtree(...)`.
* Indirekte Muster wie `os.listdir(assert_safe_path(...))` beziehungsweise `safe_path = assert_safe_path(...); os.listdir(safe_path)` wurden für die relevanten Sinks entfernt.
* Runtime-Dateizugriffe für Log und FIFO wurden ebenfalls expliziter abgesichert:
  * `runtime_path_exists(...)`
  * `runtime_getsize(...)`
  * `open_runtime(...)`
  * `ensure_runtime_parent_dir(...)`
* Download-Auslieferung erneut angepasst:
  * Download-Pfad wird lokal in `_send_file(...)` kanonisiert und geprüft.
  * `isfile`, `getsize` und `open` verwenden danach denselben geprüften lokalen Pfad.
* ZIP-Erstellung und ZIP-Import weiter gehärtet:
  * `archive.write(...)`, `os.walk(...)` und `zipfile.ZipFile(...)` verwenden geprüfte lokale Pfade.
  * ZIP-Slip-Schutz bleibt aktiv.
* Interne Papierkorb- und Settings-Dateioperationen wurden von direkten `assert_safe_internal_path(...)`-Ausdrücken an Dateisystem-Sinks auf explizite sichere Wrapper umgestellt.

#### Reliability

* Leere `except: pass`-Blöcke bleiben entfernt.
* Temporäre Upload-, Editor-, ZIP- und Settings-Dateien werden weiterhin kontrolliert bereinigt.
* Symlink-Schutz bleibt aktiv, insbesondere bei Schreib-, Lösch-, Download- und Traversal-relevanten Pfaden.

#### Maintenance

* Keine CodeQL-/LGTM-Suppression-Kommentare verwendet.
* Keine Alert-Unterdrückung eingebaut.
* Die Sicherheitslogik wurde näher an das von CodeQL empfohlene Muster gebracht: normalisieren, gegen erlaubten Basisordner prüfen, danach erst Dateisystemzugriff.
* Lokale Prüfung durchgeführt:
  * `python3 -m py_compile`
  * AST-Check auf leere `except: pass`-Blöcke
  * Smoke-Tests für Listing, Lesen, Upload-Schreiben, Traversal-Block, Symlink-Block, ZIP-Slip-Block und ZIP-Erstellung

### 1.6.1.4

#### Security / CodeQL

* CodeQL-Handling für `py/path-injection` im `winomc-console-server` erneut überarbeitet.
* Pfadprüfung so angepasst, dass der geprüfte Pfad für CodeQL klarer als bereinigter Wert erkennbar ist:
  * Normalisierung über `os.path.normpath(...)`
  * Symlink-Auflösung über `os.path.realpath(...)`
  * explizite Base-Prefix-Prüfung mit `startswith(...)`
  * separate Behandlung des Root-Ordners selbst
* `os.path.commonpath(...)` aus der zentralen CodeQL-relevanten Pfadprüfung entfernt, da CodeQL diese Custom-Hilfslogik nicht zuverlässig als Sanitizer erkannt hat.
* Download-Auslieferung überarbeitet:
  * validierter Download-Pfad wird nur einmal berechnet
  * `isfile`, `getsize` und `open` arbeiten danach mit demselben geprüften lokalen Pfad
  * kein erneutes Verschachteln von `assert_safe_download_path(...)` direkt im Filesystem-Sink
* Schutz gegen Path-Traversal bleibt erhalten und wurde CodeQL-freundlicher formuliert.
* Symlink-Schutz bleibt aktiv, um Ausbrüche aus erlaubten WinoMC-Verzeichnissen zu verhindern.

#### Reliability

* Datei erneut syntaktisch geprüft.
* Leere `except: pass`-Blöcke bleiben entfernt.
* Bestehende Sicherheitsprüfungen für Upload, Download, Editor, ZIP-Import, ZIP-Export, Papierkorb und Runtime-Dateien bleiben erhalten.

#### Maintenance

* Keine CodeQL-/LGTM-Suppression-Kommentare verwendet.
* Kein reines Unterdrücken von Alerts.
* Sicherheitslogik näher an das von CodeQL empfohlene Muster gebracht: erst normalisieren, dann gegen einen erlaubten Basisordner prüfen, danach erst Dateisystemzugriff.
* Version für Release `1.6.1.4` vorbereitet.

### 1.6.1.3

#### Security / CodeQL

* Echte Behebung der offenen CodeQL-Meldungen im `winomc-console-server`
* Unsichere Datei- und Pfadoperationen im Web-Dateimanager überarbeitet
* Schutz gegen Path-Traversal verbessert, insbesondere bei:
  * Datei-Downloads
  * Datei-Uploads
  * Datei-Bearbeitung
  * Datei-Löschung
  * Papierkorb-Funktionen
  * Wiederherstellung aus dem Papierkorb
  * ZIP-Import und ZIP-Export
* Pfadzugriffe werden nun konsequent gegen erlaubte WinoMC-Verzeichnisse geprüft
* Zugriffe außerhalb der freigegebenen Datenbereiche werden blockiert
* `..`-Traversal, ungültige Pfadsegmente und manipulierte Zielpfade werden abgewiesen
* Schutz gegen ZIP-Slip beim Entpacken von Archiven ergänzt
* Symlink-basierte Umgehungen im Dateimanager blockiert
* Runtime-Dateipfade für Konsole, Log und FIFO stärker eingeschränkt

#### Reliability

* Leere `except: pass`-Blöcke entfernt
* Fehlerbehandlung im `winomc-console-server` verbessert
* Relevante Fehler werden nun sauber protokolliert oder kontrolliert behandelt
* Stille Fehlerunterdrückung reduziert, um Debugging und Wartbarkeit zu verbessern

#### Maintenance

* Alte CodeQL-/LGTM-Suppression-Kommentare entfernt
* CodeQL-Probleme nicht mehr nur kommentiert, sondern technisch behoben
* Sicherheitsrelevante Hilfsfunktionen für sichere Pfadauflösung und Dateioperationen ergänzt
* Keine beabsichtigten Änderungen am normalen Verhalten der Webkonsole
* Keine Änderung an bestehenden Nutzeroptionen oder Add-on-Konfigurationen

### 1.6.1.2

* CodeQL-Pfadwarnungen (`py/path-injection`) in der Web-Konsole gehärtet.
* Pfadprüfung zentral auf `realpath` plus `commonpath` umgestellt.
* Upload, JSON-Upload, Datei-Editor, ZIP-Export/Entpacken, Papierkorb und Download-Pfade prüfen Ziele vor Dateioperationen erneut gegen erlaubte WinoMC-Root-Verzeichnisse.
* Add-on-Version auf 1.6.1.2 angehoben, damit Home Assistant die Sicherheitskorrektur als Update erkennt.

### 1.6.1.1

* Fehler bei der Erstellung der `allowlist.json` behoben.
* `ALLOW_LIST_USERS` im Format `Gamertag:XUID` wird jetzt korrekt in getrennte `name`- und `xuid`-Felder geschrieben.
* Name-only-Einträge wie `Gamertag` bleiben als Fallback weiterhin möglich.
* Verbindungsabbrüche mit Bedrock-Fehlerdetails wie `Boat` oder `Spyglass` bei aktivierter Allowlist behoben.

### 1.6.1

* Klassische Dateiexplorer-Ansicht korrigiert: Ordner- und Dateiliste besitzen jetzt eine eigene Scrollhöhe.
* Tabellenkopf der Dateiliste bleibt beim Scrollen sichtbar.
* Explorer- und Editorbereich laufen nicht mehr unter die angedockte Live Console.
* Add-on-Version auf 1.6.1 angehoben, damit Home Assistant die Änderung als Update erkennt.

### 1.6.0

* WinoMC Framework- und Host-Optimierungen ergänzt, ohne spielinterne Behavior Packs oder Gameplay-Eingriffe.
* Neuer Host-Optimierungsmodus für den Bedrock-Start ergänzt: `off`, `balanced`, `host_friendly`, `low_power`, `server_priority` und `expert`.
* Bedrock Dedicated Server wird jetzt über einen WinoMC-Optimierungs-Wrapper gestartet.
* Optionale CPU-Priorität, I/O-Priorität und CPU-Affinity für hostfreundlicheren Betrieb vorbereitet.
* Optionale jemalloc-Nutzung vorbereitet; automatisch konservativ, mit Fokus auf amd64 als primäres Ziel.
* Laufzeitanalyse beim Start ergänzt, inklusive Hinweisen für hohe Sichtweite, hohe Tickdistanz und große Spielerlimits.
* aarch64 bleibt experimentell, erhält aber konservative Host-Optimierungen und klare Log-Hinweise.
* Neue Home-Assistant-UI-Optionen für WinoMC Host-Optimierung ergänzt.
* Add-on-Version auf 1.6.0 angehoben, damit Home Assistant die Änderung als Update erkennt.

### 1.5.7.6

* Klassische Ansicht erweitert: Neuer Menüpunkt **Desktop-Modus** direkt unter **Experten** ergänzt.
* Der neue Menüpunkt schaltet sofort in den Desktop-Modus, ohne vorher über die Übersicht gehen zu müssen.
* Add-on-Version auf 1.5.7.6 angehoben, damit Home Assistant die Änderung als Update erkennt.

### 1.5.7.5

* Desktop-Icon-Layer für breite Home-Assistant-Ingress-Ansichten korrigiert.
* Der Desktop nutzt jetzt auch oberhalb des bisherigen kleinen Breakpoints ein relatives, stabiles Icon-Grid.
* Der scheinbar blockierende dunkle Hintergrund in größeren Browserfenstern wird dadurch vermieden.
* Desktop-Icons bleiben sichtbar und anklickbar, ohne das Browserfenster verkleinern zu müssen.
* Add-on-Version auf 1.5.7.5 angehoben, damit Home Assistant die Änderung als Update erkennt.

### 1.5.7.4

* Desktop-Workbench-Viewport korrigiert: Der Desktop-Modus belegt jetzt den kompletten sichtbaren Ingress-Bereich und erzeugt keinen zusätzlichen Root-Scrollbereich mehr.
* Alte gespeicherte Desktop-Fensterpositionen werden einmalig bereinigt, damit kein altes Übersicht-Fenster den Desktop großflächig verdeckt.
* Desktop-Fenster werden beim Öffnen und bei Größenänderungen wieder in den sichtbaren Bereich geklemmt.
* Die Übersicht wird im Desktop-Modus nicht mehr automatisch als großes Fenster geöffnet; der Desktop startet wieder frei nutzbar mit Icons und Taskleiste.
* Add-on-Version auf 1.5.7.4 angehoben, damit Home Assistant die Änderung als Update erkennt.

### 1.5.7.3

* Vor dem Start der Webconsole wird die eingebettete Datei winomc-console-server automatisch und idempotent gepatcht.
* Alte has-maximized-card / .card.maximized Overlay-Zustände werden entfernt.
* Das Desktop-Layer darf keine Klicks mehr blockieren.
* Desktop-Fenster, Icons, Taskbar, Startmenü und Konsole bleiben klickbar.
* Der Patch läuft nur beim Start und erkennt selbst, ob er schon vorhanden ist.

### 1.5.7.2

* Desktop-Overlay-Fix ergänzt
* Stale Maximized-Card-Overlays blockieren den Desktop-Modus nicht mehr
* Desktop-Workbench bereinigt beim Öffnen automatisch alte klassische Maximieren-Zustände
* Desktop-Icons und Fenster bleiben auch bei großen Viewports bedienbar

### 1.5.7.1

* Web-Schutz erweitert: Download und ZIP-Download werden jetzt ebenfalls gesperrt, solange der Web-Schutz aktiv ist
* Web-Schutz-Zustand wird zusätzlich per Cookie an das Backend gespiegelt, damit direkte Download-Endpunkte ebenfalls blockiert werden
* Seitenscrollbar-Hotfix ergänzt: Kopfzeile bleibt einzeilig, Scrollbar-Platz wird stabil reserviert und die klassische Ansicht springt nicht mehr nach unten

### 1.5.7

* Eingeklappte Navigation als Icon-Leiste korrigiert
* Menüicons bleiben bei geringer Höhe erreichbar und scrollbar

* Klassische Layout-Abstände für Übersicht, Dashboard und Dateimanager weiter bereinigt
* Dateimanager um Web-Schutz ergänzt: Änderungen, Upload, Entpacken, Löschen, Wiederherstellen und Export-Schreibaktionen können in der Weboberfläche gesperrt werden
* Web-Schutz klar benannt: betrifft nur die Weboberfläche, nicht die Server-Schreibrechte
* Papierkorb-Aufbewahrung persistent einstellbar: nie automatisch, 3, 7, 14, 30, 60 oder 90 Tage
* Papierkorb-Vorschau klarer vom Editor getrennt: Vorschau leeren / Vorschau Vollbild statt Editor-Aktionen
* Papierkorb-Dateien bleiben schreibgeschützt, bis sie wiederhergestellt werden
* Marketplace-/Pack-Hinweis verständlicher formuliert
* Dashboard-Kommandopalette stark erweitert um weitere Bedrock-Admin-Befehle mit gezielten Abfragen für Spielername, Menge, Koordinaten, Item, Effekt und weitere Parameter
* Dashboard-Kacheln und Aktionsbuttons weiter vereinheitlicht

### 1.5.6

* Mein Dashboard überarbeitet: keine lange Aktionsliste mehr, stattdessen kompakte Such-/Auswahlzeile im Stil einer Command Palette.
* Dashboard-Kacheln optisch vereinheitlicht und Ausführen-Schaltflächen konsistenter ausgerichtet.
* Papierkorb-Dateien sind im Editor nur noch als Vorschau geöffnet und können nicht mehr direkt gespeichert werden.
* Schreibzugriff auf den Papierkorb zusätzlich im Backend blockiert.
* Eingeklappte Sidebar in der klassischen Ansicht stabilisiert, damit Icons nicht mehr aus dem sichtbaren Bereich gescrollt werden.
* Abstände und Bedienflächen im Dateimanager weiter bereinigt.

### 1.5.5

* Klassische Ansicht weiter bereinigt und die Serversteuerung auf die Übersicht beschränkt
* Eingeklappte Navigation verbessert: echter Icon-only-Modus ohne horizontales Wegscrollen
* Live Console im Desktop-Modus vereinfacht: Fenstersteuerung über Minimieren, Maximieren und Schließen; Einklappen im Desktop-Modus entfernt
* Fehler behoben, bei dem die Desktop-Konsole zu klein wurde und nicht mehr sinnvoll bedienbar war
* Editor-Vollbild verbessert: Bedienleiste bleibt oben erreichbar und wird nicht mehr von der Live Console verdeckt
* Dateimanager erweitert: Löschen in einen WinoMC-Papierkorb ergänzt
* Papierkorb-Bereich ergänzt und automatisches Bereinigen alter Papierkorb-Inhalte vorbereitet
* Papierkorb kann über die Web Console geleert werden
* Wiederherstellen aus dem Papierkorb ergänzt
* Konfliktbehandlung beim Wiederherstellen ergänzt, falls am Originalpfad bereits wieder eine Datei oder ein Ordner existiert
* Papierkorb-Icon im Desktop-Modus ergänzt
* Direkter Papierkorb-Zugriff im klassischen Dateimanager ergänzt
* Explorer um zusätzlichen Aktualisieren-Button im Explorer-Bereich ergänzt
* Dateiliste wird nach relevanten Dateiaktionen zuverlässiger aktualisiert
* Persönliches Dashboard neu strukturiert: aktive Kacheln und verfügbare Aktionen sind getrennt
* Bereits hinzugefügte Dashboard-Aktionen werden im Hinzufügen-Bereich ausgeblendet
* Standard-Dashboard entschlackt und kritische Aktionen nicht mehr vorausgewählt
* Abstände, Button-Ausrichtung und Informationsblöcke in der Web Console vereinheitlicht
* Hintergrund- und Scrollverhalten in der klassischen Ansicht verbessert

### 1.5.4

* Desktop-Workbench-Konsole überarbeitet
* Live Console im Desktop-Modus als echtes bewegliches Fenster nutzbar gemacht
* Konsole kann im Desktop-Modus verschoben, minimiert, maximiert und über die Taskleiste wiederhergestellt werden
* Konsolenposition und Größe werden im Browser gespeichert
* Fehler behoben, bei dem die Live Console nach dem Einklappen im Desktop-Modus leer bzw. statisch wirkte
* Minimieren-Schaltfläche für die Live Console ergänzt
* Mobile Darstellung der Desktop-Konsole weiter abgesichert
* Klassische Ansicht bereinigt: Desktop-Fensterleisten werden dort nicht mehr angezeigt
* Mein Dashboard in der klassischen Ansicht stabilisiert
* Falsch platzierte Minimieren-/Maximieren-/Schließen-Schaltflächen in der klassischen Ansicht entfernt
* Alte dauerhaft ausgeblendete Karten werden automatisch wiederhergestellt
* Desktop-Fenster schließen Inhalte nicht mehr dauerhaft aus der Oberfläche aus
* Maximieren-Overlay von Kartenmodulen abgesichert
* Layout-Reset löscht jetzt auch gespeicherte Desktop-Konsolenpositionen

### 1.5.3

* Neuer optionaler Desktop-Workbench-Modus ergänzt
* Desktop-Verknüpfungen für Übersicht, Dateien, Konsole, Server, Packs, Backups, Netzwerk, Dashboard und Experten ergänzt
* Startmenü mit allen Web-Console-Modulen ergänzt
* Taskleiste mit offenen und minimierten Fenstern ergänzt
* Module können im Desktop-Modus als Fenster geöffnet, verschoben, minimiert, maximiert und geschlossen werden
* Fensterpositionen und geöffnete Desktop-Module werden im Browser gespeichert
* Mobile Desktop-Ansicht erkennt schmale Geräte und nutzt Fullscreen-Module statt frei schwebender Fenster
* Eingeklappte Sidebar auf echten Icons-only-Modus korrigiert
* Classic-Ansicht bleibt weiterhin verfügbar

### 1.5.2

* Web Console zur `WinoMC Dynamic Workbench` weiterentwickelt
* Sidebar einklappbar gemacht, Desktop mit Icon-only-Modus und mobilem Scroll-Menü
* Live Console dynamisch gemacht: Höhe per Drag-Leiste veränderbar, einklappbar und maximierbar
* Dateimanager modernisiert: Explorer-/Editor-Bereich per Split-Griff vergrößerbar
* Editor-Komfort ergänzt: Vollbild, Schriftgröße, Zeilenumbruch an/aus und weiterhin JSON-Prüfung vor dem Speichern
* Module/Karten können eingeklappt, maximiert oder ausgeblendet werden
* Workbench-Layout wird im Browser gespeichert und kann per Button zurückgesetzt werden
* Mobile Ansicht für iPhone, Tablet und schmale Displays verbessert
* Micro-Interaktionen und visuelle Rückmeldungen für eine modernere Bedienung ergänzt

### 1.5.1

* Web Console zum WinoMC Control Center erweitert
* Menüstruktur neu geordnet: Übersicht, Mein Dashboard, Konsole, Server, Packs & Add-ons, Backups, Netzwerk, Dateien und Experten
* Bisheriges personalisierbares Dashboard in **Mein Dashboard** umbenannt
* Neue globale **Übersicht** als feste Startseite für alle Nutzer ergänzt
* Eigene **Konsole**-Seite ergänzt; die angedockte Live Console bleibt weiterhin dauerhaft unten sichtbar
* Neues **Server**-Modul für Schnellaktionen, Spielmodus, Schwierigkeit, Spielregeln und Mehrspieler-Aktionen ergänzt
* Neues **Packs & Add-ons**-Modul für Resource Packs, Behavior Packs, Importordner und Weltdateien ergänzt
* Neues **Backups**-Modul mit sicherem Welt-Export vorbereitet
* Neues **Netzwerk**-Modul mit IPv4-, IPv6-, LAN- und Dual-Stack-Hinweisen ergänzt
* Neuer **Dateimanager** in der Web Console ergänzt
* Dateimanager unterstützt erlaubte WinoMC-Roots: `/config`, `/config/worlds`, `/config/resource_packs`, `/config/behavior_packs`, `/config/world_templates`, `/share/winomc/import` und `/share/winomc/export`
* Klickbarer Breadcrumb-Pfad ergänzt, z. B. `config:/worlds/world/db`
* Direkte Pfadeingabe mit Button **Pfad öffnen** ergänzt
* Datei-Download ergänzt
* Ordner-Download als ZIP ergänzt
* ZIP-Erstellung nach `/share/winomc/export` ergänzt
* Drag-&-Drop-Upload robuster gemacht
* Upload-Fix für Umgebungen ergänzt, in denen Home Assistant Ingress keinen normalen `Content-Length`-Header weitergibt
* JSON-Fallback-Upload für kleinere Dateien ergänzt
* Integrierter Texteditor für freigegebene Textdateien ergänzt
* JSON-Prüfung vor dem Speichern von `.json`-Dateien ergänzt
* Automatisches Datei-Backup vor dem Speichern ergänzt
* Sicheres Entpacken von `.zip`, `.mcpack`, `.mcaddon`, `.mcworld` und `.mctemplate` ergänzt
* Schutz vor Pfad-Ausbruch, Symlinks und unsicheren Archivpfaden ergänzt
* Sicherer Welt-Export mit `save hold`, `save query`, ZIP-Erstellung und anschließendem `save resume` ergänzt
* Neue optionale Limits für Web-Console-Dateifunktionen ergänzt

### 1.5.0

* Hinzufügen von neuen Funktionen für die Web Console

### 1.4.8

* Bereinigung und Dokumentation

### 1.4.7

* Experimentelle Unterstützung für `aarch64` / ARM64 ergänzt
* Add-on-Architektur um `aarch64` erweitert
* Box64 wird bei ARM64-Builds installiert
* Native Runtime erkennt ARM64 zur Laufzeit und startet den Mojang Bedrock Dedicated Server über `box64`
* Neue Option `USE_BOX64` ergänzt
* `amd64` bleibt unverändert und startet weiterhin direkt über `./bedrock_server`
* Vorbereitung für spätere eigene WinoMC-ARM64-Base fortgesetzt

### 1.4.6

* Native Unterstützung für `ENABLE_BDS_V6BIND_FIX` ergänzt
* WinoMC-eigener IPv6-Bind-Fix wird jetzt aus Quellcode im Repository gebaut
* Keine externe Download-Abhängigkeit für den IPv6-Fix beim Docker-Build
* Native Runtime setzt `LD_PRELOAD`, wenn `ENABLE_BDS_V6BIND_FIX=true` aktiv ist
* Warnung ergänzt, wenn IPv4- und IPv6-Port gleich sind, der IPv6-Bind-Fix aber deaktiviert ist
* Dual-Stack-Betrieb mit gleichem IPv4-/IPv6-Port vorbereitet

### 1.4.4

* Native `MC_PACK`-Importlogik ergänzt
* Unterstützung für `.mcpack`, `.mcaddon`, `.mcworld`, `.mctemplate` und `.zip` ergänzt
* `MC_PACK` kann auf eine Datei, einen Ordner unter `/share/winomc/import` oder eine HTTP/HTTPS-URL zeigen
* `FORCE_WORLD_COPY` nativ umgesetzt
* `FORCE_PACK_COPY` nativ umgesetzt
* Native Paket-Backups vor BDS-Updates ergänzt
* `PACKAGE_BACKUP_KEEP` zur Begrenzung alter Paket-Backups nativ umgesetzt
* Weitere itzg-Komfortfunktionen in die native WinoMC Runtime übernommen

### 1.4.3

* Native Runtime weiter bereinigt
* `VERSION=EXISTING` schreibt jetzt auch `allowlist.json` und `permissions.json`, bevor der vorhandene Bedrock Server gestartet wird
* Fehlerzweig für `VERSION=EXISTING` vereinfacht
* Doppelte Log-Ausgabe bei `BDS_AUTO_UPDATE=false` entfernt
* Doppelte Erstellung von `server.properties` bei `BDS_AUTO_UPDATE=false` entfernt
* Kleine Stabilitäts- und Wartbarkeitsverbesserungen an `winomc-native-start`

### 1.4.2

Bereinigung

### 1.4.1

* Dockerfile final auf direkte Ubuntu-Basis umgestellt
* Veraltete `build.yaml`-Abhängigkeit entfernt
* Build-Probleme durch nicht gesetztes `BUILD_FROM` behoben
* Native WinoMC Runtime erfolgreich mit eigener Basis gebaut und gestartet
* Add-on-Update auf native Runtime erfolgreich getestet
* Alte itzg-/Home-Assistant-Base-Abhängigkeit weiter bereinigt

### 1.4.0

* Docker-Basis von `itzg/minecraft-bedrock-server` auf eine eigene Ubuntu-basierte WinoMC-Runtime umgestellt
* Native Runtime ist jetzt der Standard für den Bedrock Server
* `itzg`-Runtime wird nicht mehr benötigt und bei alten Einstellungen automatisch auf `native` umgeleitet
* Architektur vorübergehend auf `amd64` begrenzt, da der native Mojang Bedrock Dedicated Server ohne itzg-/Box64-Hilfen getestet wird
* Benötigte Systempakete für nativen BDS-Start direkt im WinoMC-Image ergänzt
* Vorbereitung für eine spätere eigene `ghcr.io/kasawino69/winomc-bedrock-base` fortgesetzt

### 1.3.9

Umstellung auf Native

### 1.3.8.1

Fehlerbehebungen

### 1.3.8

* `BDS_DIRECT_DOWNLOAD_URL` bleibt als optionale Expertenoption ohne Pflichtfeld erhalten
* Native WinoMC Bedrock Runtime erfolgreich startfähig gemacht
* Nativer Start lädt den offiziellen Mojang Bedrock Dedicated Server, schreibt `server.properties` und startet den Server ohne `/opt/bedrock-entry.sh`
* Native Runtime um `allowlist.json` aus `ALLOW_LIST_USERS` erweitert
* Native Runtime um `permissions.json` aus `OPS`, `MEMBERS` und `VISITORS` erweitert
* XUID-Auflösung für Permissions vorbereitet, damit Gamertags nach Möglichkeit automatisch aufgelöst werden
* Vorbereitung für die spätere Umstellung von der itzg-Basis auf eine eigene WinoMC-/Home-Assistant-Base fortgesetzt

### 1.3.7

Fehlerbehebungen

### 1.3.6

* Native WinoMC Bedrock Runtime als Vorschau vorbereitet
* Neue Option `WINOMC_RUNTIME_MODE` ergänzt
* `itzg` bleibt als Standard- und Fallback-Runtime erhalten
* Neuer nativer BDS-Downloader für den offiziellen Mojang Bedrock Dedicated Server ergänzt
* Native Runtime schreibt `server.properties` direkt aus den Home-Assistant-Add-on-Optionen
* Vorbereitung für spätere vollständige Entfernung der itzg-Runtime
* Backup-Ausschluss für interne WinoMC-BDS-Dateien ergänzt

### 1.3.5

* WinoMC Live Console weiter verbessert
* Konsole dauerhaft im unteren Bereich der Oberfläche sichtbar gemacht
* Live-Log und freie Befehlseingabe sind jetzt in allen Menüpunkten verfügbar
* Neuen Menüpunkt `Dashboard` hinzugefügt
* Individuell anpassbares Dashboard für häufig genutzte Aktionen ergänzt
* Dashboard-Kacheln können hinzugefügt, entfernt und sortiert werden
* Layout-Auswahl für das Dashboard ergänzt
* Dashboard-Einstellungen werden persistent im Browser gespeichert
* Konsolenhöhe kann zwischen kompakt, normal und groß umgeschaltet werden
* Gewählte Konsolenhöhe wird persistent gespeichert
* Benutzeroberfläche weiter auf bessere Alltagstauglichkeit und schnellere Bedienung optimiert
* Allgemeine Hinweise bereinigt
* Nicht notwendige Hinweise zu bewusst nicht übernommenen Optionen entfernt
* Kritische Hinweise bleiben nur dort erhalten, wo Befehle gefährlich, fortgeschritten oder besser über Welt-/Dateikonfiguration zu setzen sind


### 1.3.4

* WinoMC Live Console Oberfläche vollständig überarbeitet
* Neue kategorisierte Bedienoberfläche nach Minecraft-/Realm-ähnlicher Struktur ergänzt
* Kategorien für Übersicht, Allgemein, Fortgeschritten, Mehrspieler, Spielregeln, Pakete und Expertenbefehle hinzugefügt
* Schnellbefehle für häufig genutzte Server- und Spielregel-Einstellungen verbessert
* Eingabedialoge für Befehle mit Spielername, Nachricht oder Zahlenwert ergänzt
* `save hold`, `save query` und `save resume` aus der normalen Schnellleiste entfernt
* Backup-Befehle in einen separaten Expertenbereich verschoben
* Warnhinweise für potenziell kritische Konsolenbefehle verbessert
* Nicht sinnvolle Live-Console-Optionen wie Seed, Bonustruhe, Startkarte, Welterstellung, Experimente und Pack-Aktivierung bewusst aus der Oberfläche herausgelassen
* Benutzeroberfläche für bessere Übersichtlichkeit, Bedienbarkeit und Alltagstauglichkeit optimiert


### 1.3.3

* Fehlerbehebung für die WinoMC Live Console
* Problem behoben, bei dem über die Web-Konsole gesendete Befehle als leerer Befehl erkannt wurden
* Übergabe von Konsolenbefehlen über Home-Assistant-Ingress robuster gemacht
* Unterstützung für Befehlsübergabe per Query-Parameter, JSON-Body, Form-Body und Plain-Text-Body verbessert

### 1.3.2

* WinoMC Live Console hinzugefügt
* Neue Home-Assistant-Ingress-Oberfläche zum Senden von Befehlen an den Minecraft Bedrock Server ergänzt
* Live-Log-Ausgabe für die Server-Konsole vorbereitet
* Schnellbefehle für häufige Serverbefehle ergänzt
* Neue Option `ENABLE_WEB_CONSOLE` zum Aktivieren oder Deaktivieren der Web-Konsole hinzugefügt
* Neue Option `WINOMC_CONSOLE_HISTORY_LIMIT` für die Anzahl der angezeigten Log-Zeilen ergänzt
* Interne Konsolen-Kommunikation über FIFO erweitert
* Dockerfile für die neue Web-Konsole angepasst

### 1.3.1

* Kleine Fehlerbehebungen

### 1.3.0

* README vollständig überarbeitet
* Alle Add-on-UI-Einstellungen ausführlich dokumentiert
* Hinweise zu möglichen Ausfüllwerten und empfohlenen Standardwerten ergänzt
* Dokumentation für Speicherorte, Worlds, Resource Packs, Behavior Packs sowie Import- und Export-Verzeichnisse ergänzt
* Hinweise zu Home-Assistant-Automationen und geplanten Neustarts ergänzt
* Erklärung zur Übergabe von Minecraft-Bedrock-Konsolenbefehlen aus Home Assistant ergänzt
* Hinweise zum vollständigen Home-Assistant-Supervisor-Add-on-Slug ergänzt
* Dokumentation für LAN-Sichtbarkeit, UDP-Port und IPv6-/Dual-Stack-Konfiguration erweitert
* Fehlerbehebungshinweise für LAN-Sichtbarkeit, Verbindung, Allowlist, Resource Packs und IPv6 ergänzt
* Backup- und Persistenzhinweise aktualisiert
* Sicherheits- und Betriebshinweise für private Server ergänzt
* Dokumentation von privaten oder lokalen Beispielwerten bereinigt

### 1.2.9.1

* Automatisierungen Fehlerbehebung

### 1.2.9

* Automatisierungen Fehlerbehebung

### 1.2.8

* Automatisierungen Fehlerbehebung

### 1.2.7

* Neue Funktionen
* Blueprints
* Automatisierungen
* Command-Übergabe aus Home Assistant an den Minecraft Bedrock Server

### 1.2.6

* Diverse Fehlerbehebungen

### 1.2.5

* Speicherstruktur für Serverdaten verbessert
* Ordner für `worlds`, `behavior_packs`, `resource_packs` und `world_templates` werden beim Start automatisch vorbereitet
* `/share/winomc/import` und `/share/winomc/export` für einfacheren Import und Export von Welten und Packs vorbereitet
* Log-Ausgabe beim Start erweitert, damit Speicherorte für Worlds, Packs und Importe leichter nachvollziehbar sind
* YAML-Einrückung der Option `FORCE_GAMEMODE` korrigiert
* Add-on-Konfiguration für sichtbare Home-Assistant-Datenordner verbessert

### 1.2.4

* Weitere Optimierungen am Bedrock-Startverhalten
* Add-on-Konfiguration bereinigt
* Vorbereitung für stabilere Datenablage im Home-Assistant-Add-on-Konfigurationsordner

### 1.2.3

* Allgemeine Optimierungen
* Kleinere Stabilitätsverbesserungen

### 1.2.2.5

* Ausführungsproblem der LAN-Funktion behoben
* Startverhalten der LAN-Sichtbarkeit verbessert

### 1.2.2.4

* Allgemeine Fehlerbehebungen
* Kleinere Korrekturen an der Add-on-Konfiguration

### 1.2.2.3

* Weitere LAN-Fehlerbehebungen
* Verbesserungen für lokale Servererkennung im Netzwerk

### 1.2.2.2

* Verschiedene Fehlerbehebungen für LAN-Sichtbarkeit und IPv6
* Verbesserungen für Dual-Stack-Umgebungen

### 1.2.2.1

* Fehlerbehebungen für IPv6
* Verbesserungen der IPv6-Port-Konfiguration

### 1.2.2

* Mehrere Fehlerbehebungen für IPv6
* Verbesserte Unterstützung für IPv6- und Dual-Stack-Netzwerke

### 1.2.1

* Konfigurationsnamen korrigiert von @felixstorm
* `send-command` korrigiert von @felixstorm

### 1.2.0

* Native Home-Assistant-Backups behoben
* **Breaking Change:** Serverdaten werden jetzt im Home-Assistant-Add-on-Konfigurationsordner gespeichert
* Das Add-on versucht, bestehende Daten automatisch an den neuen Speicherort zu migrieren
* Vor dem Update wird empfohlen, bestehende Serverdaten manuell zu sichern

### 1.1.0

* Laden der HAOS-Optionen behoben

### 1.0.3

* Option `SERVER_AUTHORITATIVE_MOVEMENT` behoben

### 1.0.2

* Option `ENABLE_LAN_VISIBILITY` hinzugefügt

### 1.0.1

* Optionen werden automatisch als Umgebungsvariablen gesetzt
* Add-on-Name geändert

### 1.0.0

* Erste Veröffentlichung
