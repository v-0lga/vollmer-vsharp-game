# Fortschritt: Vollmer Messe-Game
| | |
|-|-|
| **Konzept:** | [Vollmer Messe-Game](00001-c-vollmer-messe-game.concept.md) |
| **Letzte Änderung:** | 2026-09-21 |

Arbeitspakete aus Kap. 5 des Konzepts. Diese Tabelle ist die verbindliche Arbeitsliste für die Umsetzung.
Status: `⬜ offen` / `🔄 in Arbeit` / `✅ erledigt` / `⛔ blockiert`

| AP | Beschreibung | Status | Erledigt am | Nachweis / Nächster Schritt |
|-|-|-|-|-|
| AP-01 | Lokales Windows-Betriebsprofil und technische Checkliste festhalten. | ⬜ offen | | Nächster Umsetzungsschritt: Betriebsprofil aus Kap. 3.7 übernehmen, konkrete Gerätedaten ergänzen; kein Freigabeworkshop. |
| AP-02 | Reproduzierbares Projektgerüst und lokalen Offline-Host aufsetzen. | ⬜ offen | | Nach AP-01: Host und aktiven Client auf demselben Convertible ohne WLAN starten. |
| AP-03 | Autoritativen Runden-, Punkte-, Rollen- und Synchronisationskern implementieren. | ⬜ offen | | Nach AP-02: Timer, Fehl-/Schaftabzug, Deduplizierung und passive Fernclients nachweisen. |
| AP-04 | Tap-Laser, Säge-Swipe und humorvolle Retro-Editor-Hülle prototypisch umsetzen. | ⬜ offen | | Nach AP-03: Trefferzonen, Full-HD-Skalierung und lokalen Kabel-Watcher prüfen. |
| AP-05 | Inhalte, Audiopools, Musikmischer, Highscores und Lead-/Gewinnspielerfassung ausbauen. | ⬜ offen | | Nach AP-04: variable Success-/Fail-Pools, Zwei-Effekt-Limit, Ducking, Datenschutz-Overlay, Kontaktformular und geschützten Export implementieren. |
| AP-06 | Messehärtung und verbindlichen Kabel-Fallback auf Originalhardware nachweisen. | ⬜ offen | | Nach AP-05: reale Hardware, Produktionssounds und Datenschutzangaben prüfen; Netz-aus-/Kabel-/Dauerlauftest. |

## Verbindliche Paketdefinitionen

Unverändert aus Kap. 5 des Konzepts; die obere Tabelle hält ausschließlich Bearbeitungsstatus und nächsten Schritt fest.

| AP | Ziel / Scope | Betroffene Komponenten | Abhängigkeiten | Validierung | Done-Kriterium |
|-|-|-|-|-|-|
| AP-01 | Lokales Windows-Betriebsprofil und technische Checkliste festhalten. | Convertible-/Monitorprofil, Full-HD-Annahme, Anschlüsse, Audioausgang, schlanke Medien-/Datenschutzliste | Festlegungen aus Kap. 3.7 und 3.8; kein OP-05-Gate | Checkliste gegen Konzept prüfen; noch fehlende Gerätedaten explizit in OP-01 belassen | Umsetzbares Einzelgeräte-/Kabelprofil liegt vor; Restnachweise sind AP-06 zugeordnet, kein Freigabeworkshop erforderlich. |
| AP-02 | Reproduzierbares Projektgerüst und lokalen Offline-Host aufsetzen. | .NET-Solution, ASP.NET Core, Blazor-Betriebshülle, TypeScript/Phaser, Windows-Paketierung | AP-01 | sauberer Build; Kaltstart mit deaktiviertem WLAN und ohne WAN | Ein Startablauf öffnet Host und aktiven Client auf demselben Convertible sowie optional einen lokalen Kabel-Watcher. |
| AP-03 | Autoritativen Runden-, Punkte-, Rollen- und Synchronisationskern implementieren. | C#-Domäne, SignalR, Trefferzonen, Konfiguration, Snapshots, MSTest | AP-02 | Tests für 60-s-Standard, konfigurierbare Dauer, Fehl-/Schaftabzug, Rundengrenzen, Deduplizierung und passive Fernclients | Genau ein lokaler Client ist aktiv; jede Geste zählt höchstens einmal, negative Punkte beenden keine Runde und Fernclients bleiben passiv. |
| AP-04 | Tap-Laser, Säge-Swipe und humorvolle Retro-Editor-Hülle prototypisch umsetzen. | Phaser, Bohrer/Fräser mit PKD-Varianten und Schaftzonen, Sägen, Parallaxe, F-Tasten, lokale Watcher-Ansicht | AP-03 | Touch-, Zonen-, Full-HD-/DPI-, Verdeckungs- und lokale Latenztests | Gesten und Schaftabzug sind eindeutig; lokale Bildrückmeldung erfüllt das Budget, Kabel-Watcher zeigt dieselbe Runde ohne Eingaben. |
| AP-05 | Inhalte, Audiopools, Musikmischer, Highscores und Lead-/Gewinnspielerfassung ausbauen. | Manifest, Success-/Fail-Pools, Zufallsbeutel, Ducking, Kanalgrenzen, Datenschutz-Overlay/-Konfiguration, Ranglisten, Kontaktformular und Betreiberexport | AP-04; Audio-Platzhalter zulässig, Lieferstatus aus OP-07 bleibt sichtbar | Audio-Burst-/Poolgrößen-/Ducking-, Overlay-, Konfigurationswechsel-, Persistenz-, Preisberechtigungs-, Einwilligungs-, Export- und Löschtests | Rundenfluss, zwei Effekte maximal, Musik-Ducking, Hinweis ohne Neubau, Fantasienamen ohne E-Mail und Preisvergabe nur mit E-Mail sowie getrennte Vertriebsfreigabe sind umgesetzt. |
| AP-06 | Messehärtung und verbindlichen Kabel-Fallback auf Originalhardware nachweisen. | Windows-Kiosk, Monitor-/Audioumschaltung, Recovery, finale Medien, Betriebsdokumentation | AP-05; OP-01 und OP-07 abschließen; geklärte Vorgaben aus OP-03/OP-04 umsetzen | Kaltstart ohne Netz, Kabel-Hotplug, optionaler Funkverlust, vier Stunden Dauerlauf, Hörcheck und Datenschutzprüfung | Offline- und Kabelbetrieb sind nachgewiesen, Produktionssounds übernommen/geliefert/geprüft, Datenschutzhinweis konfiguriert und Nachmesse-Auswertung sowie Wiederanlauf geprobt. |

## Konzeptstand und Wiedereinstieg

- 2026-09-21: Konzept überarbeitet, keine Implementierung begonnen und kein Arbeitspaket abgeschlossen. OP-02/OP-03/OP-04/OP-05 geklärt. Nutzer bestätigt Eigen-/royaltyfree Medien und frei verwendbare Gestaltung ohne eigene Vorgabe. Lead-Erzeugung, Fantasienamen ohne E-Mail und Preisbenachrichtigung nach der Messe mit E-Mail sind festgelegt.
- Soundkonzept: je Laser/Säge `success` und `fail`, sprechende Dateinamen, 3–6 Varianten pro Aktion als Produktionsziel bei technisch variabler Anzahl. Kap. 3.4.4 benennt für jede Gruppe Ort und Zeitpunkt. Startvorschlag: zehn Bestandsclips übernehmen und 22 Dateien ergänzen; keine Originaldatei wurde umbenannt.
- Dokumentprüfung: Konzept-Patches mit `git diff --check` geprüft; ein Leerraumfehler wurde korrigiert. Editor-Diagnostik ohne Befund. Der zusätzliche Node-basierte Struktur-/Linkabgleich konnte mangels installiertem Node.js nicht ausgeführt werden. Keine Anwendungs-, Hardware- oder Hörtests ausgeführt, da hier ausschließlich Konzeptarbeit stattfindet.
- Exakter nächster Umsetzungsschritt: AP-01 vor Beginn auf `🔄 in Arbeit` setzen, dann lokales Windows-/Kabelprofil als Betriebscheckliste erfassen. Die genaue Auflösung und ausstehende Produktionsmedien blockieren das Projektgerüst nicht.
- Noch offen: konkretes Gerät samt Kabel-/Audio-Nachweis (OP-01) sowie Audioübernahme, Lieferung und Hörcheck (OP-07). OP-03 und OP-04 sind keine offenen Konzeptentscheidungen mehr. Betreiberangaben, zweckbezogene Löschtermine und Teilnahmebedingungen werden im Rahmen der regulären Einrichtung in den konfigurierbaren Hinweis eingesetzt; Kontakt-/Gewinnspielfunktionen und Schutzmaßnahmen sind noch zu implementieren und zu prüfen.

Regeln:
- Vor der Bearbeitung das aktuelle Arbeitspaket auf `🔄 in Arbeit` setzen; gleichzeitig darf höchstens ein Arbeitspaket in Arbeit sein.
- `✅ erledigt` erst nach umgesetztem Code, erforderlicher Validierung und betroffenen Dokumentationsänderungen setzen. Den Test-/Build-Nachweis in der letzten Spalte festhalten.
- Bei `⛔ blockiert` Ursache, benötigte Entscheidung oder fehlende Voraussetzung sowie den exakten Wiedereinstieg in der letzten Spalte festhalten.
- Bei Unterbrechung bleibt das aktuelle Arbeitspaket `🔄 in Arbeit`; die letzte Spalte beschreibt den konkreten nächsten Schritt.
