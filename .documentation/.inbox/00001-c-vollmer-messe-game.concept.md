# Vollmer Messe-Game
| | |
|-|-|
| **Erstelldatum:** | 2026-09-09 |
| **Letzte Änderung:** | 2026-09-21 |
| **Issue:** | 00001 |

## Commit-Vorschlag für den finalen Gesamt-Commit

> **Status:** `vorläufig` — Der Vorschlag wird nach Umsetzung, Einarbeitung aller Code-Review-Findings und erfolgreicher finaler Review anhand der tatsächlich enthaltenen Änderungen aktualisiert. Es wird kein separater Konzept-Commit erstellt.

```text
[00001] Interaktives Vollmer Messe-Game bereitstellen

- Geplante Umsetzung: humorvolles Touch-Arcade-Spiel auf einem Windows-Convertible mit lokaler Kabelanzeige und optionalem Beobachter.
- Geplante Entscheidung: autoritativer ASP.NET-Core-Server, Phaser/TypeScript-Spielclients und Blazor-Betriebshülle.
- Geplante Absicherung: automatisierte Regeltests sowie Touch-, Latenz-, Last- und Hardware-Abnahme.
- Offene Punkte: konkrete Anschlüsse und Auflösung sowie Audioübernahme und fehlende Audioassets.
```

# 0. Entscheidungsvorlage

| | |
|-|-|
| **Problem** | Ein selbstironisches Retro-Fungame soll am Messestand zuverlässig ohne WLAN spielbar sein. |
| **Lösung** | Windows-Convertible mit lokalem ASP.NET-Core-Server, Phaser/TypeScript-Client und Kabelbildschirm; zusätzliche Beobachter sind optional. |
| **Entscheidungsbedarf** | Keine kreative Freigaberunde; Mediengrundlage und Lead-/Gewinnspielzweck sind geklärt, Hardwaredetails und Sounds folgen. |
| **Top-Risiko** | Ungeprüfte Touch-, Monitor- und Audioanschlüsse gefährden den verbindlichen Kabel-Fallback. |
| **Blockierende offene Punkte** | Keine für das Projektgerüst; vor Messebetrieb OP-01 Hardwaretest und OP-07 Audioübernahme/-lieferung erledigen. OP-03 und OP-04 sind geklärt. |

# 1. Kontext, Zielsetzung & Use Case

## 1.1 tl;dr
- Das Spiel soll binnen weniger Sekunden zum Mitmachen motivieren und Vollmers Kompetenz bei Schleifen, Schärfen, Erodieren und Laserbearbeitung verständlich machen.
- Eine Runde dauert standardmäßig $60\text{ s}$; $30$ bis $90\text{ s}$ bleiben als konfigurierter, abnahmefähiger Rahmen zulässig.
- Ein Windows-Convertible ist Host und einziger aktiver Touch-Eingabepunkt; ein kabelgebundener Bildschirm zeigt die Runde passiv. Ein zusätzlicher Netzwerk-Beobachter ist optional, nicht betriebsnotwendig.

## 1.2 Ziele und Nicht-Ziele

| Ziel | Beschreibung |
|-|-|
| Z-01 | Messebesucher erkennen ohne Einweisung den Unterschied zwischen präzisem Laser-Tap und kraftvollem Schleif-Swipe. |
| Z-02 | Start, vollständige Wertungsrunde, Rangliste, Audio und Kabelanzeige funktionieren ohne Internet, WLAN oder externen Server. |
| Z-03 | Werkzeuge, Effekte, Texte, Schwellenwerte und Audio sind über versionierte Konfiguration und Assets austauschbar. |
| Z-04 | Der Watcher verstärkt das Geschehen, ohne eine zweite Eingabequelle oder einen zweiten Spieler zu erzeugen. |
| Z-05 | Der Aufbau ist vom Standpersonal mit einem definierten Startablauf und Fallback bedienbar. |
| Z-06 | Das Spiel erzeugt Leads und Gesprächsanlässe mit potenziellen Kunden; eine E-Mail-Adresse ermöglicht die Preisbenachrichtigung nach der Messe. |

| Nicht-Ziel (Scope) | Begründung |
|-|-|
| NZ-01 | Keine realistische CNC-/Maschinensimulation. | Die kurze Messeinteraktion priorisiert Verständlichkeit und Reaktionsfreude. |
| NZ-02 | Keine cloudabhängigen Konten, Online-Bestenlisten oder Telemetrie. | Der Messebetrieb muss ohne Internet auskommen und Datenschutzaufwand klein halten. |
| NZ-03 | Kein gleichzeitiges Mehrspieler-Spiel. | Genau eine aktive Rolle verhindert widersprüchliche Touch-Eingaben und vereinfacht den Betrieb. |
| NZ-04 | Keine produktive Asset-Erstellung in diesem Konzept. | Soundzuordnung und Lieferdateien werden konkret festgelegt; fehlende Medien werden nicht als vorhanden ausgegeben. |

## 1.3 Zielgruppen- und Messekontext

Primäre Nutzer sind vorbeigehende Fachbesucher und Begleitpersonen mit unterschiedlichen Vorkenntnissen. Die Interaktion muss sowohl im Vorbeigehen lesbar als auch ohne Erklärung spielbar sein. Sekundäre Nutzer sind Zuschauer am großen Bildschirm und das Standpersonal, das Startbereitschaft, Lautstärke und Störungen steuert.

Der Ablauf beginnt im Attract-Mode: bewegte Werkzeugsilhouetten und eine kurze, rein visuelle Demonstration von Tap und Swipe ziehen Aufmerksamkeit an. Audio startet erst nach bewusster Browserinteraktion; danach begleitet 90er-Arcade-Musik das Spiel. Ein großer Startbereich beginnt die Runde. Eine kurze Vorführung während der ersten Ziele ersetzt eine Textanleitung. Nach dem Ergebnis kehrt das System automatisch zum Attract-Mode zurück.

**Tonalität:** Das ist bewusst ein Fungame und eine Überraschung, kein Produktfreigabeprojekt. Selbstironie, überzeichnete Werkstückstimmen und ein liebevoll-romantischer Blick auf die historische Retro-Bedienoberfläche sollen vermeintlich bessere Zeiten spielerisch neu beleben. Kein Marketing-/Fachworkshop und keine formale Humorfreigabe sind vorgesehen. Technische Funktionsprüfungen und die tatsächlichen gesetzlichen Pflichten bleiben davon getrennt.

**Begriffe:** „Handheld“ bezeichnet in den folgenden Layoutabschnitten die aktive Touchansicht auf demselben Convertible, kein zusätzlich benötigtes Gerät. „Watcher“ bezeichnet eine passive Browseransicht, lokal auf dem Kabelmonitor oder optional auf einem weiteren Gerät.

# 2. Analyse / Ist-Zustand

## 2.1 tl;dr
- Das Repository enthält Dokumentation, Arbeitsvorgaben, ein Anschauungspaket und zehn MP3-Dateien unter [fx/audio](../../fx/audio); Anwendung und Buildkonfiguration fehlen noch.
- Die Dokumentationsstruktur war leer; [00000-issues.md](../00000-issues.md), dieses Konzept und die Fortschrittsseite bilden den ersten dokumentierten Arbeitsstand.
- Der bestehende Stack-Favorit bleibt bestehen; geändert wird die Topologie zu Server und aktivem Client auf demselben Windows-Convertible.

## 2.2 Repository-Vorgaben und Lücken

[.github/copilot-instructions.md](../../.github/copilot-instructions.md) verlangt deutsche Dokumentation und Kommentare, transparente Angaben zu fehlenden Punkten, konkrete Empfehlungen für Entscheidungen/Risiken und enge, nachweisbare Validierung. Der Skill [concept-plan](../../.github/skills/concept-plan/SKILL.md) schreibt die Kapitel 0 bis 8, den Index und die Fortschrittsseite vor; [implementation-workflow](../../.github/skills/implementation-workflow/SKILL.md) erklärt diese Fortschrittsseite für die spätere Umsetzung zur verbindlichen Arbeitsliste. Bei C#-Tests gilt [csharp-mstest](../../.github/skills/csharp-mstest/SKILL.md).

Es existieren noch keine Anwendungsprojekte, Anwendungstests oder Build-/Deployment-Skripte. Das Anschauungspaket und `yell01.mp3` bis `yell10.mp3` sind Medienbestand, keine fertige Anwendung. Die globale [.gitignore](../../.gitignore) bleibt unverändert. Der Nutzer bestätigt ausschließlich royaltyfree verwendbare oder eigene Kreationen/Kompositionen und übernimmt Pitching/Bearbeitung selbst. Damit ist OP-03 geklärt; selbst gewählte Ergänzungen müssen ebenfalls frei für den vorgesehenen Einsatz verwendbar sein.

# 3. Lösungskonzept / Soll-Zustand

## 3.1 tl;dr
- Der aktive Client bestätigt Berührungen lokal in höchstens $50\text{ ms}$; der Server bestätigt Wertung und Rundenstatus verbindlich.
- Laserziele verlangen einen Tap, Schleifziele einen gerichteten Swipe; Form, Farbe, Bewegung und Effekt unterscheiden die Bearbeitungsarten sichtbar.
- Der Spielkern bleibt datengetrieben: Tooldefinitionen, Spawnmuster, Modi, Punkte, Texte, Audio und Layoutvarianten liegen außerhalb der Kernlogik.

## 3.2 Nicht-Ziele

| Nicht-Ziel (Implementierung) | Begründung |
|-|-|
| NZ-05 | Blazor rendert nicht den Game-Loop. | DOM-basierte UI-Updates sind für Partikel, Canvas und hochfrequente Touch-Animationen nicht die passende Verantwortung. |
| NZ-06 | Kein Peer-to-Peer-Fallback für wertungsrelevante Runden. | Ohne zentrale Autorität sind Rollen, Timer und Punktestand nicht belastbar lösbar. |
| NZ-07 | Keine frei editierbaren Laufzeitskripte am Messestand. | Änderungen sollen sicher über geprüfte Konfigurationen erfolgen. |

## 3.3 Spielablauf und Kernmechaniken

1. **Standby/Attract:** Der Watcher zeigt eine großformatige Endlosszene; das Handheld zeigt einen eindeutigen Startbereich. Nach Inaktivität startet die Szene erneut.
2. **Start und Onboarding:** `F2 LASER` oder `F3 SCHEIBE` bestimmt vor dem Start verbindlich den Rundenmodus; genau einer der beiden Modi ist aktiv. `F1 START` reserviert die aktive Rolle und startet die gewaehlte Runde. Die ersten zwei Ziele demonstrieren je nach Modus Tap-Laser oder Swipe-Schleifen mit verlangsamter Bewegung und klarer Geste.
3. **Hauptphase:** Stumpfe Werkzeuge erscheinen in definierten Bahnen, Winkeln und Geschwindigkeiten. Schwierigkeit steigt über Spawnrate, Geschwindigkeit, Mischungen und kürzere Reaktionsfenster, nicht über versteckte Regeln.
4. **Feedback:** Jede abgeschlossene erfolgreiche oder nicht erfolgreiche Spielaktion erhält sichtbares Feedback und eine Audioantwort gemäß Kap. 3.4. Falsches Schärfen und Laser-Treffer auf den Schaft kosten Punkte; die Runde läuft unabhängig vom Punktestand weiter. Es gibt weder Leben noch ein vorzeitiges Game-over.
5. **Ende:** Der Server beendet nach standardmäßig $60\text{ s}$ die Wertung, beide Ansichten zeigen Ergebnis und die Rangliste des gespielten Modus. Dauer und Punktewerte sind konfigurierbar. Das Formular erfasst Name und E-Mail für Leads und die Preisbenachrichtigung nach der Messe. Wer keine Kontaktdaten angeben möchte, trägt einen Fantasienamen ohne E-Mail ein und kann seinen Highscore trotzdem speichern. Ohne erreichbare E-Mail-Adresse gibt es keinen Preis; die Teilnahme am Spiel und die Ranglistenanzeige bleiben möglich. `Ohne Eintrag weiter` verwirft das Formular. Der konfigurierbare Datenschutzhinweis erläutert die Zwecke vor dem Speichern (Kap. 3.8).
6. **Rückkehr:** Nach konfigurierbaren $10$ bis $20\text{ s}$ ohne Eingabe oder nach Abschluss der Namenseingabe wird die Rolle freigegeben und der Attract-Mode geladen.

**Rollenwechsel:** Die reguläre Spieleransicht erhält kein frei sichtbares Rollenmenü. Am Watcher beziehungsweise Betreiber-Host öffnet ein unauffälliger, dokumentierter Langdruck einen Betreiber-Dialog. Der Dialog zeigt den verbundenen aktiven Client und bietet die Aktionen `Rolle freigeben` sowie `Runde abbrechen und Rollen freigeben`. Beide Aktionen verlangen einen bewusst ausgeführten Slide-to-confirm-Schalter. Während einer laufenden Runde ist nur die zweite Aktion verfügbar, damit eine Wertung nicht still an eine andere Person übergeht. Nach einer Freigabe oder einem Abbruch kehren alle Clients zum Attract-Mode zurück; der nächste Handheld-Tap kann die aktive Rolle übernehmen. Der Server führt die bereits definierte atomare Übergabe mit erhöhter `RoleVersion` aus.

**Laser-Modus:** Bohrer und Fräser unterschiedlicher Größe, teilweise mit PKD (polykristallinem Diamant), besitzen jeweils einen sichtbaren Schaft. Das Werkzeugmanifest enthält getrennte Polygone für bearbeitbare Schneiden und Schaft; Größenvarianten skalieren Bild und Zonen gemeinsam. Ein Tap auf eine Schneide ist erfolgreich, ein Tap auf den Schaft ist eine Fehlbearbeitung mit Punktabzug. An einer gemeinsamen Zonengrenze hat der Schaft Vorrang; es gibt genau ein Ergebnis pro Geste. Cyanfarbene Zielmarkierungen und ein punktförmiger Lichtimpuls machen die gültige Zone lesbar. Die hochgepitchten Stimmen geben den Werkstücken den gewünschten comicartigen, an „Worms“ erinnernden Charakter; Originalspiel-Aufnahmen sind damit nicht vorausgesetzt.

**Schleif-Modus:** Sägeblätter sind die primären Werkstücke und erhalten die noch zu liefernden tiefer gepitchten Stimmen. Ein Swipe entlang der markierten Schneide löst eine virtuelle Schleifscheibe und gerichtete Funken aus. Ein falscher Winkel, eine zu kurze Geste oder eine Bearbeitung des Blattkörpers zählt als falsches Schärfen. Richtung und Mindeststrecke sind sichtbar und tolerant konfigurierbar; weitere Werkzeugtypen benötigen erst eigene Zonen und Soundzuordnungen.

**Bevorzugte Startwerte für die Punktebalance:** `roundDurationSeconds: 60` (zunächst validierter Bereich 30 bis 90), `correctScore: 10`, `wrongSharpeningPenalty: -5`, `shaftHitPenalty: -10`, `missPenalty: -5`. Negative Gesamtstände sind zulässig. Die Combo ist vorerst nur Anzeige, kein Multiplikator. Ein unbearbeitet vorbeiziehendes Werkzeug verursacht keinen Abzug und keinen Interaktionssound. Diese Werte sind ein konkreter konfigurierbarer Entwurf, keine fachliche Freigabeanforderung.

Eine Spielaktion ist genau eine im Canvas begonnene und abgeschlossene Tap-/Swipe-Geste, nicht jeder Pointer-Move. Pro Geste wird höchstens ein Werkzeug ausgewertet: zuerst die früheste getroffene Zone entlang des Pfads, bei Gleichstand die vorderste sichtbare Zone. Der falsche Gestentyp auf einem Werkzeug zählt als `wrong_sharpening`; im Laser-Modus hat ein erkannter Schaftkontakt Vorrang. Eine Geste ohne Werkzeugkontakt zählt als `miss`. Bereits vollständig geschärfte Werkzeuge werden aus den Trefferzonen entfernt. Abgebrochene Pointer-Gesten, gesperrte Bedienelemente und Eingaben nach Rundenende sind keine neuen wertungsrelevanten Aktionen. Ein Rundentimer wird durch Overlays nicht angehalten.

## 3.4 Grafik-, Asset- und Soundkonzept

Die visuelle Richtung verbindet die historische gelbe Vollmer-Arbeitsfläche, das anthrazitfarbene Gehäuse und die grünen/blauen Retro-Bedienelemente mit einer comicartigen Produktionswelt. Der Wiedererkennungswert und die liebevolle Überzeichnung sind ausdrücklich erwünscht. Werkzeuge benötigen klar lesbare Silhouetten und die Zustände `dull`, `in_progress` und `sharp`; fotorealistische Details sind nur dort sinnvoll, wo sie die Form nicht verschleiern.

**Farbe und Schrift:** Vorgaben des Nutzers haben Vorrang. Ohne solche Vorgaben werden eine eigenständig gewählte Farbpalette und frei für den vorgesehenen Einsatz verwendbare Schriften eingesetzt, bevorzugt lokal gebündelte OFL-Schriften. Keine kostenpflichtige Corporate-Schrift und kein fremdes restriktiv lizenziertes Gestaltungspaket werden vorausgesetzt. Erforderliche Lizenzhinweise bleiben im Paket; eine zusätzliche Designfreigabe ist nicht vorgesehen.

Das HUD bleibt am Handheld kompakt: oben Zeit und Punkte, darunter Combo und Modusindikator. Der Watcher nutzt dieselben Ereignisse, zeigt aber großformatige Werkzeugbewegungen, den aktuellen Spielerstatus, Punkte, Combo und optional die Tagesrangliste. Der Watcher enthält keine Eingabecontrols.

Empfohlene Laufzeitstruktur: `Assets/images`, `Assets/animations`, `Assets/audio`, `Assets/fonts`, `Assets/templates` und `Assets/config`. Jedes Asset besitzt eine monotone, pro Kategorie eindeutige Manifest-ID, etwa `tool_0001` oder `sfx_0001`; eine entfernte ID bleibt reserviert. Audiodateinamen beschreiben die Aktion nach dem Schema `<aktion>_<nummer>.<format>`, beispielsweise `laser_success_01.mp3`. [fx/audio](../../fx/audio) bleibt die Eingangsablage. Die vorhandenen `yell`-Originaldateien bleiben während der Konzeptarbeit unverändert; die geplante Übernahme unter sprechenden Liefer-/Laufzeitnamen steht in Kap. 3.4.1. Die Runtime verwendet die Manifestzuordnung, nicht eine aus Dateinamen abgeleitete Spiellogik. Weitere Varianten benötigen keinen Codeumbau.

Die fachliche Bedeutung liegt ausschließlich im versionierten Manifest, beispielsweise `tool_0001` mit `toolType: "drill"`, `interaction: "tap"`, Schneiden-/Schaftpolygonen, `states: ["dull", "sharp"]` und Audiogruppen. Eine Assetvorlage pro Kategorie dokumentiert Pflichtfelder, Formate, Pivot, Auflösung, Varianten, Performancegrenzen und Fallbacks. Ein Manifest-Validator prüft vor jeder Runde IDs, referenzierte Dateien und Vorlagenkonformität; bei Fehlern bleibt das letzte gültige Manifest aktiv. Fehlt auch dieses, startet nur ein ausdrücklich gekennzeichneter Demo-Modus. PNG/WebP eignet sich für Rastergrafiken, JSON-Atlanten für Sprites. Die gelieferten MP3s werden direkt unterstützt und vor Spielbeginn dekodiert; OGG bleibt optional, WAV eignet sich für kurze Feedbacksignale. Kein CDN und kein Streaming sind erforderlich.

| Dateiname | Zweck und Anzeigeort | Format und technische Anforderungen | Austauschbar über Konfiguration | Erwartete Varianten |
|-|-|-|-|-|
| `images/tools/tool_0001.png` | Werkzeugbild in Handheld und Watcher | PNG/WebP mit Transparenz, Quelle mindestens 1024 px längste Kante, Vorlage `tool-image` | ja | `dull`, `sharp`, Werkzeugtyp |
| `animations/laser/fx_0001.json` | Laserblitz | Phaser-kompatibler Partikel-/Sprite-Atlas, Vorlage `laser-effect` | ja | Intensität, Farbe, Dauer |
| `animations/grind/fx_0001.json` | Schleiffunken | Atlas/JSON mit begrenzter Partikelzahl, Vorlage `grind-effect` | ja | Richtung, Dichte, Material |
| `images/ui/attract_0001.webm` | Attract-Hintergrund am Watcher | lokales WebM, stumm oder mit separatem Audio, Vorlage `watcher-video` | ja | Hochformat, Querformat |
| `audio/laser/laser_success_01.mp3` | erfolgreiche Laseraktion | MP3, bereits hochgepitcht, Vorlage `laser-sfx` | ja | Pools `laser_success` und `laser_fail`, jeweils 3–6 angestrebt |
| `audio/grind/saw_success_01.mp3` | erfolgreiche Sägeaktion | MP3, bereits tiefer gepitcht, Vorlage `grind-sfx` | ja | Pools `saw_success` und `saw_fail`, jeweils 3–6 angestrebt |
| `audio/music/arcade_music_01.mp3` | 90er-Arcade-Hintergrundmusik | MP3, Loopbereich im Manifest, Vorlage `music-loop` | ja | ein Startstück genügt, Musik ist kein Aktionspool |
| `audio/ui/round_end_01.mp3` | Rundenende | MP3, kurzer Jingle, Vorlage `ui-sfx` | ja | Pools `round_start` und `round_end`, jeweils 3–6 angestrebt |

Audio ist in `Assets/audio/{ui,laser,grind,ambient,music}` organisiert. Ein Audioprofil legt getrennte Lautstärken für Musik, kurze Feedbacksignale und Werkstückstimmen fest. Die folgende Zuordnung ist die konkrete Liefer- und Implementierungsvorgabe; sie ist noch keine vorhandene Laufzeitimplementierung. Audio darf eine sichtbare Aktion nie blockieren. Herkunft und Nutzungsrechte werden schlank im Manifest dokumentiert, ohne zusätzlichen kreativen Freigabeprozess.

### 3.4.1 Konkrete Zufallszuordnung der Werkstückstimmen

**Bestand und Übernahme:** `yell01.mp3` bis `yell10.mp3` sind unter [fx/audio](../../fx/audio) vorhanden. Der Nutzer pitcht und bearbeitet selbst und verwendet ausschließlich royaltyfree verwendbare Quellen oder eigene Kreationen/Kompositionen (OP-03 geklärt). Geplant ist die Übernahme von `yell01.mp3` bis `yell05.mp3` nach `laser_success_01.mp3` bis `laser_success_05.mp3` und von `yell06.mp3` bis `yell10.mp3` nach `laser_fail_01.mp3` bis `laser_fail_05.mp3`, jeweils in derselben Reihenfolge. Die sprechenden Zieldateien sind noch nicht angelegt. Die inhaltliche Auswahl kann beim Hörcheck getauscht werden; hier wurde kein Klang oder Pegel geprüft.

| Modus / Audiopool | Exakte Bedingung | Punkte (Startwert) | Konkrete Startdateien, gleiche Gewichtung | Klang / Status |
|-|-|-|-|-|
| LASER / `laser_success` | Tap trifft bearbeitbare Schneide von Bohrer oder Fräser, mit oder ohne PKD | +10 | `laser_success_01.mp3`, `laser_success_02.mp3`, `laser_success_03.mp3`, `laser_success_04.mp3`, `laser_success_05.mp3` | hochgepitchter Jubel; fünf Bestandsclips übernehmen |
| LASER / `laser_fail` | Falsche Geste, Werkzeugkörper, Schafttreffer oder Geste ohne Werkzeugkontakt | -5; Schaft -10 | `laser_fail_01.mp3`, `laser_fail_02.mp3`, `laser_fail_03.mp3`, `laser_fail_04.mp3`, `laser_fail_05.mp3` | hochgepitchter Protest; fünf Bestandsclips übernehmen |
| SCHEIBE / `saw_success` | Gültiger Swipe entlang der Sägezahnschneide | +10 | `saw_success_01.mp3`, `saw_success_02.mp3`, `saw_success_03.mp3` | tiefer gepitchter zufriedener Ausruf; zu liefern |
| SCHEIBE / `saw_fail` | Falsche Richtung, zu kurzer Swipe, Tap, Blattkörper oder Geste ohne Werkzeugkontakt | -5 | `saw_fail_01.mp3`, `saw_fail_02.mp3`, `saw_fail_03.mp3` | tiefer gepitchter Protest; zu liefern |

**Poolgröße:** Pro Aktion sind 3–6 Dateien das Produktionsziel, keine technische Mindest-/Höchstgrenze. Alle tatsächlich konfigurierten Varianten nehmen an der Zufallsauswahl teil; auch ein oder mehr als sechs Clips funktionieren ohne Codeänderung. Die Startliste verwendet fünf Varianten je Laserpool und drei je weiterem Aktionspool. Weitere Dateien folgen dem gleichen Namen mit `_04`, `_05`, `_06` usw. Fehlbearbeitung, Schaft und Fehlschlag bleiben für Punkte und Bildfeedback unterscheidbar, teilen aber den jeweiligen `fail`-Soundpool.

Jeder Pool verwendet einen zufällig gemischten Beutel: Alle vorhandenen Varianten werden einmal gezogen, danach neu gemischt; bei mehr als einer Variante keine unmittelbare Wiederholung an der Beutelgrenze. Ein Ein-Datei-Pool spielt seine einzige Datei; ein leerer Pool nutzt den definierten Audio-Fallback. Gemeinsam verwendete Pools teilen den Beutel. Ziehung nur beim tatsächlichen Soundstart, keine Häufung durch verworfene Ereignisse. Keine zusätzliche Laufzeit-Pitchänderung der bereits bearbeiteten Lieferdateien. Empfohlene Sprachclips: 0,3 bis 1,2 Sekunden, maximal 1,8 Sekunden, ohne lange führende Stille, ohne Musikbett, mit einheitlicher wahrgenommener Lautheit und ohne Clipping. Längere Bestandsdateien benötigen einen konfigurierten, hörgeprüften Ausschnitt.

### 3.4.2 Feedbacksignale, Bedienaktionen und Musik

Alle hier genannten Dateien fehlen noch. Sie werden ebenfalls unter `fx/audio/` geliefert, im Release aber nach `Assets/audio/ui/` beziehungsweise `Assets/audio/music/` kopiert. Auch diese Aktionspools verwenden 3–6 Varianten als Ziel und dieselbe anzahlunabhängige Zufallsauswahl. Nur die durchgehende Hintergrundmusik benötigt keine 3–6 Dateien.

| Aktion / Ereignis | Exakte Datei(en) | Auslösung und Klangauftrag |
|-|-|-|
| Jede erfolgreiche Spielgeste | `feedback_success_01.wav`, `feedback_success_02.wav`, `feedback_success_03.wav` | sehr kurzer positiver Arcade-Impuls (30 bis 50 ms), zusätzlich zur Werkstückstimme |
| Jede fehlgeschlagene Spielgeste, einschließlich Schaft und Fehlschlag | `feedback_fail_01.wav`, `feedback_fail_02.wav`, `feedback_fail_03.wav` | unterscheidbarer negativer Arcade-Impuls (30 bis 50 ms), zusätzlich zur Werkstückstimme |
| Gültige UI-Aktion | `ui_confirm_01.mp3`, `ui_confirm_02.mp3`, `ui_confirm_03.mp3` | zufälliger Retro-Klick, höchstens 120 ms; Moduswahl, F4/F5, Datenschutz öffnen/schließen, Speichern, `Ohne Eintrag weiter`, bestätigte Betreiberaktion |
| Abgelehnte UI-Aktion | `feedback_fail_01.wav`, `feedback_fail_02.wav`, `feedback_fail_03.wav` | gemeinsamer Fehlerpool bei ungültiger Formulareingabe oder nicht zulässigem Befehl; keine Punktewirkung |
| `F1 START` akzeptiert | `round_start_01.mp3`, `round_start_02.mp3`, `round_start_03.mp3` | zufälliger Start-Jingle, höchstens 800 ms; ersetzt den UI-Klick |
| Reguläres Rundenende | `round_end_01.mp3`, `round_end_02.mp3`, `round_end_03.mp3` | zufälliger Abschluss-Jingle, höchstens 1.200 ms; kein zusätzlicher Combo-/Highscore-Jingle |
| Jede akzeptierte rechte Ereignistaste | `ui_confirm_01.mp3`, `ui_confirm_02.mp3`, `ui_confirm_03.mp3` | gemeinsamer UI-Pool; kein zusätzliches Sprach-/Gag-Audio und keine Punktewirkung |
| `F6 TON` | `ui_confirm_01.mp3`, `ui_confirm_02.mp3`, `ui_confirm_03.mp3` | gemeinsamer UI-Pool beim Wechsel in laut/leise im neuen Pegel; Wechsel in stumm bleibt lautlos |
| Hintergrundmusik | `arcade_music_01.mp3` | ein durchgehender 90er-Arcade-Loop ohne Gesang; gewünschte Lieferung 60 bis 120 s plus Loopmarken |

Scrollen im Overlay, Tippen in Textfelder, Pointer-Moves, automatische Zielbewegungen und Combo-Anzeigen lösen keine zusätzlichen Sounds aus. Gemeint ist eine abgeschlossene Spielgeste oder ein ausgelöster UI-Befehl, nicht jedes technische Eingabeereignis. F6 stumm, die Systemstummschaltung und ein defekter Audiopfad sind explizite Ausnahmen vom hörbaren Feedback; sichtbare Rückmeldung bleibt immer erhalten.

### 3.4.3 Mischer, Ducking und Begrenzung

**Favorit:** Ein gemeinsamer Web-Audio-Mischer des aktiven Clients mit Musikbus, einem Kurzfeedbackkanal und genau einem Sprach-/Jinglekanal. Shell und Phaser verwenden denselben Mixer; optionale Watcher bleiben stumm. Damit gibt es höchstens zwei Effektsounds plus einen Musikstream gleichzeitig, kein zusätzliches Ambientbett und kein Audioecho von einer zweiten Anzeige.

- Jede abgeschlossene Spielaktion startet ihr kurzes Erfolg-/Fehlersignal. Bei Überlappung beendet ein 3-ms-Fade das vorige Kurzsignal und startet das neue; keine Warteschlange. UI-Klicks verwenden denselben Kurzfeedbackkanal, niemals einen zusätzlichen Kanal. Extrem schnelle Eingaben können akustisch verschmelzen, erzeugen aber keine anwachsende Soundmenge.
- Werkstückstimmen starten frühestens 180 ms nach dem letzten Sprachstart. Während dieser Sperre bleibt nur das jeweils jüngste Ereignis vorgemerkt; es verfällt nach 200 ms und wird bei Rundenende entfernt. Der Kurzimpuls bestätigt auch die Aktionen, deren Stimme entfällt. Neue zulässige Stimmen ersetzen die laufende Stimme mit 15-ms-Fade-out und anschließendem Start, niemals mit überlappendem Crossfade. Keine wachsende Queue und keine nachträgliche Stimmenkaskade.
- Start-/End-Jingles haben Vorrang vor Werkstückstimmen, ersetzen sie und löschen vorgemerkte Stimmen. Das Endereignis ist einmalig; bei gleichzeitigem Treffer und Rundenende bleibt der Treffer-Kurzimpuls hörbar, der End-Jingle übernimmt den Sprachkanal. Dekorative Ereignisse dürfen Wertungsfeedback nicht verdrängen und bleiben während einer Spielgeste akustisch nachrangig.
- Musik wird bei jedem Effekt innerhalb von 20 ms um 12 dB abgesenkt. Nach Ende des letzten Effekts folgen 120 ms Haltezeit und 250 ms Rückkehr zum Normalpegel. Neue Effekte verlängern die Absenkung, addieren aber keine weiteren -12 dB. F6 und Masterpegel multiplizieren den gesamten Mix; Ducking darf eine Stummschaltung niemals aufheben.
- Startprofil: Musik -18 dB, Werkstückstimmen/Jingles -6 dB, Kurzfeedback -12 dB, Master-Limiter als Clippingschutz. Alle Pegel, Sperrzeiten, Clipausschnitte, Poolzuordnungen, Loopgrenzen und Duckingwerte sind validierte Konfiguration und werden zwischen Runden aktiviert.
- Der erste bewusste Start-Tap entsperrt den AudioContext. Vorher ist Attract still; danach läuft der Loop in der Runde und optional mit weiteren -6 dB im Attract-/Ergebniszustand. Vorladen und Dekodieren geschehen vor Rundenstart. Ein Browser-Autoplayfehler wird im Betreiberstatus sichtbar.
- Fachliche Sounds werden erst durch das bestätigte `ActionResolved` ausgelöst, lokal vorhergesagte Effekte bleiben rein visuell. `EventId` verhindert doppelte Wiedergabe; Snapshot/Replay und Watcher-Reconnect spielen keine historischen Sounds ab. Am lokalen Host beträgt das Ziel für bestätigte Audioantworten höchstens 100 ms p95 vom Gestenende bis zum Audio-Start.
- Fällt eine Sprachdatei aus, bleibt das kurze Erfolg-/Fehlersignal. Fehlt auch dessen Datei, erzeugt Web Audio einen kurzen konfigurierten Ersatzton; dieser ist ausdrücklich kein Produktionsasset. Ohne Audiogerät bleibt der Ablauf sichtbar spielbar und meldet den Fehler dem Betreiber. Eine fehlende Musikdatei führt zu Stille auf dem Musikbus, nicht zum Abbruch der Runde.

### 3.4.4 Konkrete Lieferliste

Die Anzahl ist der konkrete Startvorschlag; 3–6 Varianten pro Aktion sind gewünscht, andere Poolgrößen ändern die Technik nicht. Alle Sounds kommen ausschließlich aus dem aktiven Convertible-Client über dessen gewählten Audioausgang; lokale und entfernte Watcher bleiben stumm.

| Lieferung / Aktion | Exakte Startdateinamen | Anzahl | Wo und wann wird abgespielt? | Status |
|-|-|-:|-|-|
| Laser / Success | `laser_success_01.mp3`, `laser_success_02.mp3`, `laser_success_03.mp3`, `laser_success_04.mp3`, `laser_success_05.mp3` | 5 | Im Laser-Spiel nach bestätigtem Tap auf die bearbeitbare Schneide; zufällige Stimme im Sprachkanal | aus `yell01.mp3` bis `yell05.mp3` übernehmen; Zieldateien noch nicht angelegt |
| Laser / Fail | `laser_fail_01.mp3`, `laser_fail_02.mp3`, `laser_fail_03.mp3`, `laser_fail_04.mp3`, `laser_fail_05.mp3` | 5 | Im Laser-Spiel nach Schafttreffer, falscher Bearbeitung oder Fehlschlag; gemeinsamer zufälliger Fehlerpool im Sprachkanal | aus `yell06.mp3` bis `yell10.mp3` übernehmen; Zieldateien noch nicht angelegt |
| Säge / Success | `saw_success_01.mp3`, `saw_success_02.mp3`, `saw_success_03.mp3` | 3 | Im Säge-Spiel nach bestätigtem gültigem Schneiden-Swipe; zufällige Stimme im Sprachkanal | zu liefern, tiefer gepitcht |
| Säge / Fail | `saw_fail_01.mp3`, `saw_fail_02.mp3`, `saw_fail_03.mp3` | 3 | Im Säge-Spiel nach falschem Swipe, Tap, Blattkörpertreffer oder Fehlschlag; zufällige Stimme im Sprachkanal | zu liefern, tiefer gepitcht |
| Kurzfeedback / Success | `feedback_success_01.wav`, `feedback_success_02.wav`, `feedback_success_03.wav` | 3 | In beiden Spielmodi bei jeder bestätigten erfolgreichen Geste, zusätzlich zur Stimme im Kurzfeedbackkanal | zu liefern oder selbst zu produzieren |
| Kurzfeedback / Fail | `feedback_fail_01.wav`, `feedback_fail_02.wav`, `feedback_fail_03.wav` | 3 | In beiden Spielmodi bei jeder bestätigten Fehlaktion; außerdem bei abgelehntem UI-Befehl oder ungültiger Formulareingabe im Kurzfeedbackkanal | zu liefern oder selbst zu produzieren |
| UI / Bestätigung | `ui_confirm_01.mp3`, `ui_confirm_02.mp3`, `ui_confirm_03.mp3` | 3 | In der Betriebshülle bei akzeptierter Moduswahl, F4/F5, Info öffnen/schließen, Speichern/Überspringen, Ereignistasten, Betreiberaktion und F6 laut/leise | zu liefern oder selbst zu produzieren |
| Rundenstart | `round_start_01.mp3`, `round_start_02.mp3`, `round_start_03.mp3` | 3 | Einmal nach angenommenem F1-Start im Sprach-/Jinglekanal, statt UI-Klick | zu liefern oder selbst zu produzieren |
| Rundenende | `round_end_01.mp3`, `round_end_02.mp3`, `round_end_03.mp3` | 3 | Einmal beim serverbestätigten regulären Rundenende im Sprach-/Jinglekanal, mit Vorrang vor Stimmen | zu liefern oder selbst zu produzieren |
| Arcade-Musik | `arcade_music_01.mp3` | 1 | Nach Audiofreigabe durch Start-Tap durchgehend im Musikbus; während der Runde, optional leiser in Attract/Ergebnis, bei Effekten geduckt | zu liefern |

Der Startvorschlag umfasst **32 Laufzeitdateien**: zehn Laserclips aus dem Bestand und **22 neu zu liefernde oder zu produzierende Dateien**. Weitere Varianten sind willkommen, aber keine technische Voraussetzung. Keine separaten Schaft-/Fehlschlagpools, Ambientloops oder Gag-Sounds nötig. Die Hörzuordnung des Laserbestands wird bei der Übernahme geprüft; diese Konzeptänderung benennt die Originaldateien noch nicht um.

Ein Anschauungspaket liegt unter [demo-material](../demo-material/README.md). Es dient ausschließlich der Abstimmung. **Seine technische und gestalterische Qualität ist ausdrücklich kein Maßstab für die Produktionsqualität des Games.** Seine früheren Beispieldateinamen sind keine zusätzliche Audiolieferliste; dafür gilt Kap. 3.4.4. Das Release benötigt ein validiertes Manifest, nutzbare Medienrechte und funktionierende Qualitäts-/Performanceprofile auf der Finalhardware, keinen separaten kreativen Freigabeprozess.

## 3.5 Hintergrund, räumliche Tiefe und Humor

**Favorit:** Eine stilisierte, ruhig automatisch scrollende Produktionslandschaft mit drei Parallaxebenen. Das erzeugt die räumliche Spannung bekannter Arcade-Sidescroller, ohne eine hektische Moorhuhn-Kopie oder eine technische Maschinensimulation zu werden. Die Welt bewegt sich horizontal, die Kamera bleibt ruhig; Besucher müssen weder scrollen noch eine Kamera steuern.

| Ebene | Parallaxfaktor | Inhalt | Spielregel |
|-|-:|-|-|
| Hintergrund | $0{,}15$ bis $0{,}25$ | Hallenstruktur, entfernte Maschinen, Lichtbaender und abstrakte Markenflaechen | rein dekorativ, nie Zielverdeckung |
| Mittelebene | $0{,}55$ bis $0{,}70$ | Werkzeuge, Zufuehrung, Bearbeitungszellen und Qualitaetsstationen | alle Ziele und Trefferzonen |
| Vordergrund | $1{,}00$ bis $1{,}25$ | Schutzscheibe, Kabeltraeger, Transportkorb, Foerdergut und Spänefilter | zeitlich begrenzte, regelbasierte Teilverdeckung |
| HUD | $0$ | Zeit, Punkte, Combo und Modus | nie von Weltobjekten verdeckt |

Der Hintergrund ist eine **Mischform aus prozeduraler Komposition und konkreten Assets**. Wiederholbare Hallenmodule, Bodenraster, Lichtlaeufe und dezente Partikel entstehen aus konfigurierten Kacheln und Farbwerten; sie eignen sich fuer endloses Scrolling und schnelle Layoutvarianten. Werkzeuge, Schutzvorrichtungen, charakteristische Maschinenmerkmale und verwendete Bildzeichen sind als PNG-Master und WebP-Auslieferungsasset vorgesehen. Laser, Funken, Schleifspuren und kleine Ereignisse verwenden Atlas-/JSON-Animationen mit PNG/WebP-Texturen. Das verhindert sowohl generische prozedurale Werkzeuge als auch eine unflexible, vollstaendig vorgerenderte Panoramawelt.

Verdeckungen sind bewusstes Gameplay, keine zufaellige Renderfolge: Jedes Ziel ist mindestens $650\text{ ms}$ voll sichtbar, maximal $40\,\%$ seiner Trefferflaeche darf fuer hoechstens $450\text{ ms}$ verdeckt sein, und die relevante Schneiden- beziehungsweise Tap-Zone bleibt sichtbar. In den ersten $15\text{ s}$ der Runde gibt es keine Verdeckung; zwischen zwei verdeckten Zielen liegen mindestens $2\text{ s}$. Eine ausbleibende Aktion auf ein verdecktes Ziel kostet weder Punkte noch Combo. Diese Regeln erhalten den humorvollen "gerade noch erwischt"-Moment, ohne die Messeinteraktion unfair zu machen.

Humor prägt Werkstückstimmen, Retro-Hülle und die gesamte Inszenierung. Zusätzliche kurze Gags in der industriellen Welt bleiben spielregelneutral. Beispiele sind eine uebermotiviert vorbeifahrende Qualitaetsmarkierung, ein zu schnell rollender Schraubenkorb, ein kurz aufblinkendes "scharf genug"-Pruefsiegel oder eine Schutzscheibe, die ein Ziel theatralisch freigibt. Jeder Gag hat ein eigenes sichtbares Ereignis, eine Manifest-ID, ein konfiguriertes Spawnprofil und eine optionale Audioereignisgruppe. Er darf weder Trefferzonen veraendern noch notwendige Gesten verschleiern und ist jederzeit per Konfiguration deaktivierbar. Das Startpaket benötigt dafür keine zusätzlichen Sounds über Kap. 3.4 hinaus.

| Neue Assetkategorie | Zweck | Pflichtfelder im Manifest |
|-|-|-|
| `background-module` | Kachelbares Hallen-, Boden-, Licht- oder Maschinenmodul | `layer`, `parallaxFactor`, `tileWidth`, `palette`, `spawnProfile` |
| `foreground-occluder` | zeitweise sichtbarer Vordergrundgegenstand | `maxCoveredTargetArea`, `maxDurationMs`, `cooldownMs`, `motionProfile` |
| `tool-overlay` | stumpfe Schneide, Fortschritt oder geschärfte Kante | `toolId`, `state`, `anchor`, `blendMode` |
| `humor-event` | rein dekorative, spielregelneutrale Nebeninszenierung | `trigger`, `cooldownMs`, `visualAssetId`, `audioEvent`, `enabled` |
| `event-sfx` | gewichtete, kurze Audioantwort auf ein Ereignis | `event`, `variants`, `volumeProfile`, `fallback` |
| `ambient-loop` | dezente Hallen- oder Maschinenatmosphaere | `zone`, `loopPoints`, `volumeProfile`, `fallback` |

Die Zuordnung von Bild zu Ton erfolgt ausschließlich ueber das Manifest, nie ueber Dateinamen oder feste Codepfade. Ein `humor-event` referenziert beispielsweise `audioEvent: "occluder_reveal_comic"`; diese Ereignisgruppe verweist auf eine oder mehrere gewichtete `event-sfx`-IDs. Fehlt die Gruppe oder eine Datei, bleibt die sichtbare Nebeninszenierung erhalten und der Runtime-Fallback bleibt stumm. Damit koennen Content-Verantwortliche neue Gags, Hintergrundvarianten und Tonvarianten per neuem Asset plus validiertem Manifesteintrag aktivieren, ohne die Kernlogik anzupassen.

## 3.6 Retro-Editor-Betriebshuelle und Spielinszenierung

**Favorit:** Die historische Vollmer-Editorreferenz wird als klar erkennbare, modern touchfaehige Betriebshuelle nachgebildet. Das feste Anthrazitgehause, die gelbe Arbeitsflaeche, die linke Maschinen-/Achsspalte, die rechte grune Ereignisleiste und die untere F-Tastenleiste rahmen das Spiel, ersetzen aber weder die Phaser-Spiellogik noch die direkte Geste auf dem Werkzeug.

| Bereich | Aufgabe | Verbindliche Regel |
|-|-|-|
| Gelbe Arbeitsflaeche | Einziger Spielcanvas fuer Ziele, Treffer, Partikel und Parallaxwelt | Kein Bedienelement verdeckt die Flaeche; die Touchgeste findet ausschliesslich hier statt. |
| Linke Statusspalte | Kompakte Retro-Anzeige fuer Programm, Punkte, verbleibende Zeit, Combo und virtuelle Achsen | Achswerte sind rein visuelles Feedback und werden aus bestaetigten oder lokal vorhergesagten Aktionen animiert; sie sind keine Eingabe. |
| Linke Maschinenflaeche | Comicartige Bearbeitungszelle mit sichtbarer Aufhaengung fuer Laser und Schleifscheibe | Die Maschine bleibt ausserhalb der Trefferzone und zeigt nur nachvollziehbare, kurze Bewegungen zur jeweiligen Aktion. |
| Rechte Ereignisleiste | Acht grune Symboltasten fuer harmlose Nebeninszenierungen | Waerend einer Runde loesen Tasten nur lokale, konfigurierbare Bild-/Toneffekte ohne Punkte-, Combo- oder Rollenwirkung aus. |
| Untere Funktions- und F-Tastenleiste | Blaue Funktionsfelder ueber sechs grauen Retro-Tasten | `F1` ist ein nicht rastender Starttaster; `F2` bis `F6` sind visuell rastende Schalter. Die blauen Felder tragen die Klartextbefehle, die grauen Tasten nur `F1` bis `F6`. |

Der humoristische Sketch ist die visuelle Referenz fuer Kontrast, Proportionen und die komische Werkzeuginszenierung; die Medienherkunft wird davon getrennt dokumentiert. Die Bedienleiste verwendet keine historischen Produktionsbefehle unveraendert, damit Besucher nicht auf eine echte Maschinensteuerung schliessen. Die blauen Funktionsfelder tragen die fachlichen Bezeichnungen `START`, `LASER`, `SCHEIBE`, `ANLEITUNG`, `HIGHSCORE` und `TON`; die darunterliegenden grauen F-Tasten tragen nur `F1` bis `F6` und sind eine visuelle Retro-Metapher. `F1` ist der einzige Momenttaster und startet nur aus dem Attract- oder Ergebniszustand. `F2` und `F3` sind gegenseitig ausschliessende, rastende Modusschalter: Genau einer ist sichtbar gedrueckt und bestimmt die Runde sowie die zugehoerige Rangliste. `F4`, `F5` und `F6` sind rastende Schalter und behalten ihren sichtbaren Zustand bis zu einer erneuten Betaetigung oder bis zum konfigurierten Ruecksetzen. `F4` aktiviert die kurze, rein visuelle Geste-Demonstration. `F5` oeffnet die getrennte Rangliste des aktiven Modus; waehrend einer laufenden Runde wird sie nicht geoeffnet. `F6` schaltet den gesamten lokalen Mix aus Musik, Werkstückstimmen und Bedienfeedback zwischen laut, leise und stumm. Betreiberbefehle bleiben ausserhalb dieser Besucherleiste und folgen weiterhin dem geschuetzten Langdruck mit Slide-to-confirm.

Die Ranglisten `LASER` und `SCHEIBE` sind getrennte, lokal persistent gespeicherte Bestenlisten; ein Eintrag besteht aus Modus, Punktzahl, Anzeigename, Zeitstempel, Hinweis-/Einwilligungsversion und einer technischen Eintrags-ID. Ein Fantasiename genügt. E-Mail, Gewinnspielteilnahme und gegebenenfalls Einwilligung zur Vertriebsansprache werden getrennt von der öffentlichen Rangliste gespeichert. Nur Einträge mit E-Mail und Gewinnspielteilnahme werden bei der Preisvergabe nach der Messe berücksichtigt; die öffentliche Rangliste enthält auch Einträge ohne E-Mail. Betreiber können die erforderlichen Kontakt-/Gewinnspieldaten geschützt für die Nachbearbeitung exportieren, einzelne Einträge oder beide Ranglisten löschen. Exporte sind nicht für Watcher erreichbar und unterliegen denselben Zweck- und Löschregeln. Ohne Eintrag, nach Formular-Timeout oder bei Abbruch werden keine eingegebenen Kontaktdaten gespeichert. Wiederherstellung darf gelöschte/abgelaufene Daten nicht erneut veröffentlichen; Details regelt Kap. 3.8.

Die linke Comicmaschine zeigt als technische Andeutung einen kurzen Portaltraeger mit Schlitten, Kabelschlauch und gemeinsamer Werkzeugaufnahme. Der Laser ist ein schmaler Fokuskopf unter der Aufnahme; die Schleifscheibe haengt seitlich daneben an einem kurzen Schwenkarm mit sichtbarem Schutzring. Im Laser-Modus faehrt der Schlitten entlang einer virtuellen $X$-Achse, richtet sich aus und sendet einen kurzen cyanfarbenen Strahl sichtbar in die gelbe Flaeche. Im Scheiben-Modus schwenkt der Arm vor, die Scheibe dreht an und bleibt mit ihrer Schutzhaube klar als bearbeitendes Werkzeug lesbar. Der Swipe ueber die markierte Werkzeugschneide wird als lineare Zustellung gelesen: Die $X$-Anzeige folgt der Wischrichtung, $Z$ zeigt einen kurzen Zustellimpuls, und ein schmaler Funkenstreifen laeuft entlang der Schneide. Dadurch bleibt die Ninja-Slice-Qualitaet erhalten, ohne dass Besucher eine Scheibe frei zeichnen oder die Maschine selbst steuern muessen.

Die rechten Symboltasten sind als optionale "Werkstatt-Spielereien" ausgestaltet, etwa Messuhr-Zucken, Foerderband-Kurzlauf, Schutzscheiben-Wischer, Pruefsiegel-Stempel oder ein kleiner Werkzeugwechsel. Jede Taste hat ein eindeutiges Piktogramm, einen maximal $2\text{ s}$ langen Effekt, einen konfigurierbaren Cooldown und optionalen Ton. Easter Eggs duerfen erst nach einer konfigurierten Folge harmloser Ereignistasten oder nach einem Highscore erscheinen; sie verbergen weder Ziele noch Navigation, veraendern keine Wertung und sind im Betreiberprofil global deaktivierbar. Die Taste `F6 TON` und die Systemlautstaerke gelten auch fuer alle Spielereien.

Die Referenz ist keine pixelgenaue Reproduktion: Auf Handhelds werden Lesbarkeit und Zielgroessen gegenueber dekorativer Dichte priorisiert. Die Spalten und F-Tasten werden proportional mit festen Mindestgroessen skaliert; bei schmalem Hochformat wird die rechte Ereignisleiste als ausklappbare Symbolschublade angezeigt, waehrend die gelbe Spielflaeche mindestens $70\,%$ der Breite erhaelt. Der Watcher uebernimmt die Arbeitsflaeche und eine grossformatige Maschinenanimation, zeigt jedoch weder F-Tasten noch Ereignistasten als bedienbar an.

## 3.7 Lokaler Windows-Betrieb und verbindlicher Kabel-Fallback

**Favorit:** Windows 11, lokal paketierter ASP.NET-Core-Host inklusive Client, Schriften, Assets und Konfiguration sowie installierter Chromium-basierter Browser (bevorzugt Edge). Windows 10 bleibt ein gesondert auf dem konkreten Gerät zu prüfendes Kompatibilitätsziel, keine ungeprüfte Supportzusage. Framework-, Browser- und Betriebssystemversionen werden passend zur Zielhardware festgeschrieben. Keine Installation, Lizenzaktivierung oder Assetabfrage darf beim Messe-Kaltstart Internet benötigen.

| Betriebsart | Aufbau | Verbindlichkeit |
|-|-|-|
| Einzelgerät | Server, Betriebshülle und aktiver Phaser-Client auf demselben Convertible, Kommunikation über Loopback | muss bei deaktiviertem WLAN und gezogenem Netzwerkkabel vollständig funktionieren |
| Kabelanzeige | HDMI/DisplayPort beziehungsweise passender USB-C-Adapter zum Monitor; erweiterter Windows-Desktop, zweites lokales Browserfenster als stummer Watcher | verbindlicher Fallback, kein Netzwerk und kein zweiter Rechner nötig |
| Drahtloser Beobachter | separates Gerät mit passivem Browserclient über eine vor Ort verfügbare lokale Verbindung | optional; Ausfall oder fehlende Erlaubnis der Funkverbindung verhindert keine Runde |

Die Kabelanzeige ist bewusst eine lokale Zuschaueransicht statt einer dauerhaften 1:1-Spiegelung: So bleiben Bildschirmtastatur, Datenschutzeinwilligung und E-Mail-Eingabe auf dem Convertible. Windows „Duplizieren“ ist nur für den technischen Bildtest ohne Personendateneingabe geeignet. Im regulären Betrieb wird „Erweitern“ verwendet; bei unklarer Monitorzuordnung bleiben Personendatenfelder gesperrt und das Spiel kann ohne Eintrag weiterlaufen.

**Auflösung:** Planungsbasis ist 1920 × 1080, 16:9 quer. Native Convertible-/Monitorauflösung und Windows-Skalierung werden nachgeliefert. Canvas und Trefferkoordinaten nutzen dieselbe logische 16:9-Fläche; andere Seitenverhältnisse erhalten Letterboxing statt verzerrter Werkzeuge. Touchflächen bleiben mindestens 44 × 44 CSS-Pixel groß. Full HD bei 100 %, 125 % und 150 % Windows-Skalierung ist vorläufige Layout-Prüfbasis, keine bereits bestandene Hardwareabnahme.

**Start und Fallback:** Netzteil und Kabel anschließen, Windows-Anzeigemodus prüfen, lokalen Host starten, aktive Ansicht auf dem Touchdisplay und lokalen Watcher auf dem zweiten Monitor öffnen, Tonprobe mit fest gewähltem Ausgabegerät durchführen. Fällt Funk aus, läuft die Runde weiter; der Kabel-Watcher kann sich jederzeit mit einem Snapshot zuschalten. Monitor-Abziehen oder -Anstecken darf weder aktive Rolle noch Timer ändern. HDMI/USB-C kann das Windows-Audioziel umschalten: bevorzugt bleibt der getestete lokale Lautsprecher-/Klinkenausgang fest gewählt; Monitoraudio wird nur nach eigenem Anschlusstest verwendet. Ein Audio-Selbsttest nach dem Umstecken gehört zur Betriebscheckliste.

Standardmäßig bindet der Host nur an Loopback. Eine externe Schnittstelle wird nur für den optionalen Beobachterbetrieb bewusst aktiviert, mit lokaler Firewallbegrenzung, vertrauenswürdig eingerichtetem HTTPS und kurzlebiger Watcher-Berechtigung. Fernclients erhalten serverseitig ausschließlich Leserechte, niemals eine aktive oder Betreiberrolle. Kein Besucher-WLAN und kein eigener Access Point sind Voraussetzung des Spielbetriebs.

## 3.8 Datenschutz-Infobutton und konfigurierbares Overlay

**Festgelegt:** Ein kleines `i` im Kreis in der festen Bedienleiste öffnet „Datenschutz“. Das sichtbare Symbol ist klein, seine Touchfläche mindestens 44 × 44 CSS-Pixel; zugänglicher Name und Tooltip lauten „Datenschutz“. Es ist vor der ersten Dateneingabe sowie im Highscoreformular erreichbar. Nur der aktive Client zeigt das Overlay, niemals der Watcher.

Das modale Overlay besitzt eine begrenzte Höhe innerhalb des verfügbaren Viewports, einen eigenen vertikal scrollbar bedienbaren Textbereich und eine jederzeit sichtbare Schließen-Schaltfläche. Touch-Scroll, Mausrad und Tastaturnavigation funktionieren auch bei Bildschirmtastatur und 200 % Browserzoom. Fokus bleibt im Dialog und kehrt beim Schließen zum Auslöser zurück; Escape schließt, Gesten werden nicht an den Spielcanvas durchgereicht. Der Rundentimer läuft weiter. Während einer bewussten Formular-/Overlayinteraktion läuft kein Eingabe-Timeout ab; nach Inaktivität werden ungespeicherte Eingaben verworfen.

**Konfiguration ohne Neubau:** Vorgesehen ist `Assets/config/privacy.de.json` mit `version`, `controller`, `contact`, `dataProtectionContact`, `purposes`, `legalBases`, `recipients`, `retention`, `rights`, `supervisoryAuthority` und Textabschnitten für den Hinweis. Das E-Mail-Feld ist sichtbar und aktiviert; sein Zweck ist Lead-Erfassung und Preisbenachrichtigung nach der Messe. `leaderboardDeleteAt`, `prizeDataDeleteAt`, `leadDataDeleteAt`, Teilnahmebedingungen und Einwilligungstexte werden ebenfalls konfiguriert. Keine Skripte oder beliebiges HTML, sondern UTF-8-Text und strukturierte Absätze. Neuladen validiert Pflichtfelder und aktiviert die neue Version vor dem nächsten Formular ohne Build; offene Formulare behalten ihre Version bis zum Abschluss. Ungültige Konfiguration lässt die letzte gültige Fassung aktiv. Ohne gültigen Hinweis bleibt das Spiel ohne personenbezogene Speicherung nutzbar.

**Festgelegter Ablauf:** Das Spiel dient der Lead-Erzeugung und dem Gespräch mit potenziellen Kunden. Im Ergebnisformular werden Name und E-Mail angeboten; ein Name oder Fantasiename ist für den gespeicherten Highscore erforderlich. E-Mail ist für das Spielen und die Rangliste optional, für die Preisvergabe nach der Messe dagegen erforderlich. Leere E-Mail ist beim Speichern eines reinen Highscores zulässig, eine ausgefüllte Adresse wird auf plausibles Format geprüft. Eine Formatprüfung garantiert keine Erreichbarkeit: Preise werden erst nach erfolgreicher Kontaktaufnahme vergeben. Fantasienamen sind auch mit E-Mail erlaubt; es wird kein amtlicher Name verlangt.

**Zwecke und Rechtsgrundlagen:** Die Veröffentlichung des Anzeigenamens mit dem Highscore erfolgt mit freiwilliger Einwilligung nach Art. 6 Abs. 1 lit. a DSGVO. Wer am Gewinnspiel teilnimmt, bestätigt die verlinkten, ebenfalls konfigurierbaren Teilnahmebedingungen; die dafür erforderliche E-Mail-Verarbeitung und spätere Gewinnbenachrichtigung dienen der Durchführung nach Art. 6 Abs. 1 lit. b DSGVO. Für weitergehende vertriebliche E-Mail-Ansprache gibt es eine separate freiwillige, nicht vorangekreuzte Einwilligung. Ihre Ablehnung verändert weder Spiel, Highscore noch Gewinnchance. Eine reine Gewinnspieladresse ist keine pauschale Werbeerlaubnis; der Betreiberexport unterscheidet Gewinnspielkontakte und für Vertriebsansprache freigegebene Leads. Kein Newsletter und kein automatischer E-Mail-Versand aus dem Spiel sind vorgesehen.

E-Mail und Einwilligungsnachweise erscheinen nie auf dem Watcher oder in Diagnose-Logs. Der Server speichert Zweck, Hinweisversion und Zeitpunkt; für die Löschung gibt es einen nur dem Spieler angezeigten Löschcode. `Ohne Eintrag weiter` bleibt möglich. Ein Widerruf der Vertriebsansprache beendet nur diese Verarbeitung, nicht automatisch die unabhängig davon gewünschte Gewinnspielteilnahme.

### 3.8.1 Konkreter Hinweistext als Konfigurationsvorlage

> **Datenschutz beim Vollmer Messe-Game**
>
> Verantwortlich ist [vollständige juristische Person, postalische Anschrift]. Kontakt: [Kontaktadresse]. Datenschutzkontakt beziehungsweise Datenschutzbeauftragte/r, soweit benannt: [Kontaktdaten].
>
> Mit dem Messe-Game möchten wir mit potenziellen Kunden ins Gespräch kommen. Du kannst einen Namen oder Fantasienamen angeben und auch ohne E-Mail spielen sowie einen Highscore speichern. Für die Rangliste verarbeiten wir Anzeigename, Spielmodus, Punktestand, Zeitpunkt, technische Eintragskennung und Einwilligungsnachweis. Nur Anzeigename, Punktestand und Rang sind am Messestand und auf Zuschaueranzeigen sichtbar; deine E-Mail-Adresse bleibt vertraulich.
>
> Die Veröffentlichung deines Highscores erfolgt mit deiner freiwilligen Einwilligung nach Art. 6 Abs. 1 lit. a DSGVO. Möchtest du an der Preisvergabe teilnehmen, benötigen wir eine erreichbare E-Mail-Adresse und deine Bestätigung der Teilnahmebedingungen. Wir ermitteln und benachrichtigen Gewinner erst nach der Messe und verwenden die Adresse zur Gewinnabwicklung nach Art. 6 Abs. 1 lit. b DSGVO. Ohne E-Mail-Adresse ist kein Preis möglich; spielen und ein Fantasiename in der Rangliste bleiben möglich.
>
> Nur wenn du zusätzlich zustimmst, verwenden wir deinen Namen und deine E-Mail-Adresse für die vertriebliche Kontaktaufnahme zu deinem Interesse an Vollmer-Produkten nach der Messe. Grundlage ist Art. 6 Abs. 1 lit. a DSGVO. Diese Zustimmung ist freiwillig und hat keinen Einfluss auf deine Gewinnchance. Einwilligungen kannst du jederzeit beim Standpersonal oder über [Kontaktadresse] mit Wirkung für die Zukunft widerrufen; die Verarbeitung bis dahin bleibt rechtmäßig. Mit deinem Löschcode können wir deinen Eintrag zuordnen.
>
> Wir speichern die Daten zunächst lokal auf dem Messegerät. Für Gewinnabwicklung und von dir erlaubte Vertriebsansprache übernimmt [zuständiges Team beim Verantwortlichen] die jeweils erforderlichen Daten über einen geschützten Export. Weitere Empfänger beziehungsweise eingesetzte E-Mail-Dienstleister: [konkrete Angaben]. Drittlandübermittlungen: [keine oder konkrete Angaben einschließlich Schutzmaßnahmen]. Das Spiel selbst verwendet kein Tracking und keine Cloudverbindung.
>
> Ranglistendaten löschen wir am [Datum]. Gewinnspielkontakte löschen wir nach Abschluss der Gewinnabwicklung spätestens am [Datum]; für vertriebliche Kontaktaufnahme freigegebene Leads spätestens am [Datum], sofern nicht inzwischen eine eigenständige Kundenbeziehung mit gesondertem Verarbeitungszweck entstanden ist. Einwilligungsnachweise und gegebenenfalls gesetzlich aufzubewahrende Nachweise speichern wir für [konkrete Fristen und Gründe]. Die Regeln gelten auch für Exporte und lokale Sicherungen.
>
> Du hast nach den gesetzlichen Voraussetzungen Rechte auf Auskunft, Berichtigung, Löschung, Einschränkung der Verarbeitung und Datenübertragbarkeit sowie, soweit einschlägig, Widerspruch. Du kannst dich bei [zuständige Datenschutzaufsichtsbehörde, Anschrift/Internetadresse] beschweren. Für Anfragen erreichst du uns unter [Kontaktadresse].

**Formulartexte, jeweils ohne Vorbelegung:**

- Rangliste: „Ich möchte meinen Anzeigenamen und mein Ergebnis in der am Messestand sichtbaren Rangliste speichern lassen. Diese Einwilligung kann ich jederzeit widerrufen.“
- Gewinnspiel: „Ich möchte an der Preisvergabe nach der Messe teilnehmen und akzeptiere die Teilnahmebedingungen. Dafür gebe ich eine erreichbare E-Mail-Adresse zur Gewinnbenachrichtigung an.“
- Vertriebsansprache, separat freiwillig: „Vollmer darf mich nach der Messe unter meiner E-Mail-Adresse zu meinem Interesse an Vollmer-Produkten kontaktieren. Diese Einwilligung kann ich jederzeit widerrufen.“

Der Datenschutzhinweis ist vor Speichern erreichbar; Öffnen oder Schließen des Infotexts ersetzt keine Einwilligung. Das Formular weist direkt am E-Mail-Feld darauf hin: „Ohne E-Mail kannst du mitspielen; einen Preis gibt es nur mit erreichbarer E-Mail-Adresse.“

### 3.8.2 Umsetzung der geklärten Datenschutzvorgabe

**Bevorzugtes Aufbewahrungsprofil:** Keine pauschale Löschung direkt nach Messeende, bevor Gewinner kontaktiert wurden. Ranglisten und Gewinnspielkontakte bis zum Abschluss der Preisabwicklung zuzüglich 30 Tagen vorhalten; für erlaubte Vertriebsansprache erfasste Leads ohne weiterführende Kundenbeziehung höchstens sechs Monate nach Messeende. Diese Startwerte werden als konkrete zweckbezogene Löschtermine konfiguriert; erforderliche Nachweise mit eigener begründeter Frist getrennt führen. Ein Widerruf der Vertriebsansprache ist unverzüglich zu berücksichtigen. Bei Neustart werden überfällige Daten vor Anzeige gelöscht. Gerät, Daten und Exporte durch Windows-Konto, restriktive Dateirechte und Datenträgerverschlüsselung schützen. Betreiber können einzelne oder alle Einträge löschen; Exporte und Sicherungen sind in die Löschung einzubeziehen.

**OP-04 ist fachlich geklärt.** Die Platzhalter für Verantwortlichen, Kontakt, Empfänger, Termine und Aufsichtsbehörde werden bei der Einrichtung ausgefüllt, ohne neue Konzeptentscheidung oder Neubau. Die Vorlage orientiert sich an Art. 13 der [DSGVO](https://eur-lex.europa.eu/eli/reg/2016/679/oj?locale=de); der geklärte Konzeptstatus ist keine rechtliche Zertifizierung. Hinweistext, tatsächliche Nutzung und technische Umsetzung müssen übereinstimmen.

# 4. Contracts

## 4.1 tl;dr
- Der Server ist die einzige Autorität für Rolle, Rundentimer, Punktestand und validierte Treffer.
- Clients senden versionierte Befehle, erhalten bestätigte Ereignisse und bei Reconnect vollständige Snapshots.
- Ein Watcher bleibt passiv und kann ohne Einfluss auf die Runde jederzeit wieder beitreten.

| Schnittstelle | Richtung | Datenstruktur / Protokoll |
|-|-|-|
| Sitzungsbeitritt | Client -> Server | `JoinRequest { ClientInstanceId, RequestedRole, ResumeToken? }`; lokal gleichursprünglich über Loopback, externer Watcher über HTTPS/WebSocket. Antwort enthält kurzlebige Berechtigung und `RoleVersion`; aktive Rolle nur mit lokaler Betreiber-Provisionierung, nicht allein aufgrund von `RequestedRole`. |
| Spieleraktion | Aktiver Client -> Server | `ActionRequested { RoundId, CommandSequence, ToolId, ActionKind, InputTimestamp, Gesture }`; `ActionKind` ist `tap` oder `swipe`. |
| Bestätigung | Server -> alle Clients | `ActionResolved { RoundId, EventId, StateVersion, Outcome, HitZone, AudioEvent, ScoreDelta, ToolState, ServerTimestamp }`; identisch wiederholbar. `Outcome` unterscheidet `success`, `wrong_sharpening`, `shaft_hit`, `miss`; `AudioEvent` ist je Modus ausschließlich `laser_success`, `laser_fail`, `saw_success` oder `saw_fail`. Audio nur am aktiven Client, genau einmal je `EventId`. |
| Zustandssnapshot | Server -> Client | `GameSnapshot { RoundId, StateVersion, Phase, ActiveClientId, Mode, RemainingMs, Score, Combo, Tools }`; bei Join, Reconnect und Versionslücke. |
| Highscore / Kontakt | Aktiver Client -> Server | `HighscoreSubmitted { RoundId, Name, LeaderboardConsent, PrivacyVersion, Email?, PrizeParticipation, TermsVersion?, SalesContactConsent }`; Name oder Fantasiename, Modus/Punkte aus der Serverrunde. Gewinnspielteilnahme verlangt E-Mail und bestätigte Bedingungen; Vertriebsansprache verlangt separat E-Mail und Einwilligung. Ohne beides wird keine zwecklose E-Mail gespeichert. Öffentliche Antwort nur Anzeigename, Punktzahl und Rang; Löschcode nur privat. Überspringen/Timeout speichert nichts. |
| Nachmesse-Auswertung | Betreiber -> lokaler Server | geschützter Export von Gewinnspielkontakten beziehungsweise eingewilligten Leads, einschließlich Zweck, Eintrags-ID und Hinweis-/Bedingungs-/Einwilligungsversion. Keine Watcherberechtigung; Gewinnberechtigung serverseitig prüfen, keine E-Mail in öffentlichen Ranglisten. |
| Betrieb | Blazor-Shell -> Server | lokaler Diagnose-/Resetvertrag; der Betreiber-Slide für Freigabe/Abbruch erfordert Betreiberberechtigung und erhöht `RoleVersion`, bei Abbruch zusätzlich `RoundId`. |
| Konfiguration | Host -> Clients | versionierte Gameplay-, Asset-, Audio- und Datenschutzkonfiguration: Laufzeit, Punkte, Trefferzonen, Dateipools, Zufallsregel, Mischerlimits, Ducking, Hinweistext und Löschregeln; validiertes Neuladen zwischen Runden ohne Neubau. |

Ein lokaler ASP.NET-Core-Dienst verwendet SignalR/WebSocket für Ereignisse und Snapshots. Transportzustellung ersetzt keine Fachlogik: Der Server dedupliziert `ClientInstanceId + CommandSequence`, prüft `RoundId` und aktive Rolle und beantwortet Wiederholungen idempotent. Ein atomarer Rollenwechsel entzieht zuerst die alte Rolle, erhöht `RoleVersion`, weist dann die neue Rolle zu und sendet einen Snapshot. Die Betreiberaktion `ReleaseRole` ist außerhalb einer Runde zulässig; `AbortRoundAndReleaseRole` beendet die aktive Runde ohne Highscoreeintrag.

# 5. Implementierungsplan

## 5.1 tl;dr
- Die Umsetzung beginnt mit einem lokalen Windows-Betriebsprofil und einem vertikalen Prototyp; fehlende Produktionssounds und die genaue Auflösung blockieren das Projektgerüst nicht.
- Phaser/TypeScript plus lokaler ASP.NET-Core-Server ist der Favorit, weil Touch-Reaktion und Betriebsverantwortung sauber getrennt bleiben.
- Die Retro-Editor-Huelle ist eine Clientdarstellung mit phasengebundenen Befehlen; sie aendert weder die autoritative Wertung noch die passive Watcherrolle.
- Die nachfolgenden Arbeitspakete sind verbindlich und unverändert in der Fortschrittsseite geführt.

| AP | Ziel / Scope | Betroffene Komponenten | Abhängigkeiten | Validierung | Done-Kriterium |
|-|-|-|-|-|-|
| AP-01 | Lokales Windows-Betriebsprofil und technische Checkliste festhalten. | Convertible-/Monitorprofil, Full-HD-Annahme, Anschlüsse, Audioausgang, schlanke Medien-/Datenschutzliste | Festlegungen aus Kap. 3.7 und 3.8; kein OP-05-Gate | Checkliste gegen Konzept prüfen; noch fehlende Gerätedaten explizit in OP-01 belassen | Umsetzbares Einzelgeräte-/Kabelprofil liegt vor; Restnachweise sind AP-06 zugeordnet, kein Freigabeworkshop erforderlich. |
| AP-02 | Reproduzierbares Projektgerüst und lokalen Offline-Host aufsetzen. | .NET-Solution, ASP.NET Core, Blazor-Betriebshülle, TypeScript/Phaser, Windows-Paketierung | AP-01 | sauberer Build; Kaltstart mit deaktiviertem WLAN und ohne WAN | Ein Startablauf öffnet Host und aktiven Client auf demselben Convertible sowie optional einen lokalen Kabel-Watcher. |
| AP-03 | Autoritativen Runden-, Punkte-, Rollen- und Synchronisationskern implementieren. | C#-Domäne, SignalR, Trefferzonen, Konfiguration, Snapshots, MSTest | AP-02 | Tests für 60-s-Standard, konfigurierbare Dauer, Fehl-/Schaftabzug, Rundengrenzen, Deduplizierung und passive Fernclients | Genau ein lokaler Client ist aktiv; jede Geste zählt höchstens einmal, negative Punkte beenden keine Runde und Fernclients bleiben passiv. |
| AP-04 | Tap-Laser, Säge-Swipe und humorvolle Retro-Editor-Hülle prototypisch umsetzen. | Phaser, Bohrer/Fräser mit PKD-Varianten und Schaftzonen, Sägen, Parallaxe, F-Tasten, lokale Watcher-Ansicht | AP-03 | Touch-, Zonen-, Full-HD-/DPI-, Verdeckungs- und lokale Latenztests | Gesten und Schaftabzug sind eindeutig; lokale Bildrückmeldung erfüllt das Budget, Kabel-Watcher zeigt dieselbe Runde ohne Eingaben. |
| AP-05 | Inhalte, Audiopools, Musikmischer, Highscores und Lead-/Gewinnspielerfassung ausbauen. | Manifest, Success-/Fail-Pools, Zufallsbeutel, Ducking, Kanalgrenzen, Datenschutz-Overlay/-Konfiguration, Ranglisten, Kontaktformular und Betreiberexport | AP-04; Audio-Platzhalter zulässig, Lieferstatus aus OP-07 bleibt sichtbar | Audio-Burst-/Poolgrößen-/Ducking-, Overlay-, Konfigurationswechsel-, Persistenz-, Preisberechtigungs-, Einwilligungs-, Export- und Löschtests | Rundenfluss, zwei Effekte maximal, Musik-Ducking, Hinweis ohne Neubau, Fantasienamen ohne E-Mail und Preisvergabe nur mit E-Mail sowie getrennte Vertriebsfreigabe sind umgesetzt. |
| AP-06 | Messehärtung und verbindlichen Kabel-Fallback auf Originalhardware nachweisen. | Windows-Kiosk, Monitor-/Audioumschaltung, Recovery, finale Medien, Betriebsdokumentation | AP-05; OP-01 und OP-07 abschließen; geklärte Vorgaben aus OP-03/OP-04 umsetzen | Kaltstart ohne Netz, Kabel-Hotplug, optionaler Funkverlust, vier Stunden Dauerlauf, Hörcheck und Datenschutzprüfung | Offline- und Kabelbetrieb sind nachgewiesen, Produktionssounds übernommen/geliefert/geprüft, Datenschutzhinweis konfiguriert und Nachmesse-Auswertung sowie Wiederanlauf geprobt. |

| Option | Vorteile | Nachteile |
|-|-|-|
| A: Blazor Server als vollständiger Spielclient | Durchgängig C#, zentraler Zustand | Touch und Rendering hängen an einer Dauerverbindung; für Partikel/Canvas unnötig fragil. |
| B: Reines Blazor WASM | Offlinefähiges Browser-Frontend, C#-Kenntnisse | Kein Vorteil für den Game-Loop; Mehrgeräte-Autorität bleibt zusätzlich nötig. |
| C: Blazor-Shell + ASP.NET Core + TypeScript/Phaser | Lokaler C#-Betrieb, performanter Canvas-Loop, browserfähige Handhelds, klare Verantwortung | Zwei Frontend-Toolchains benötigen festgeschriebene Builds. |
| D: Reines TypeScript/Phaser | Kleiner Spielstack | Kiosk, Diagnostik und C#-Integration müssen separat gelöst werden. |

**Favorit:** Option C.

**Begründung:** Phaser ist für Canvas, Partikel, Audio und Touch-Gesten der direkte Laufzeitfit; ASP.NET Core und Blazor geben dem überwiegend C#/C++-erfahrenen Team eine robuste Heimat für Regeln, Diagnose und lokalen Messebetrieb. Gegenüber A bleibt die unmittelbare Touch-Reaktion vom Netz entkoppelt; gegenüber D erhält der Betrieb einen klaren, wartbaren Host.

Empfohlene Werkzeuge: Phaser als 2D-Laufzeit-Engine, TypeScript für Clientcode, .NET/ASP.NET Core für Server und Shell, optional Blender für 3D-Quellmodelle, Figma oder vergleichbares Tool für UI und eine DAW für selbst produzierte Audioassets. Medienherkunft und Nutzungsrechte bleiben nachvollziehbar, ohne formale kreative Freigabeschleife. C++ bleibt für einen später nachgewiesenen nativen Algorithmus reserviert; keine Browser-DLL-Annahme.

# 6. Tests

## 6.1 tl;dr
- Der Schwerpunkt liegt auf messbaren Touch-, Synchronisations-, Performance- und Messebetriebsprüfungen auf echter Zielhardware.
- C#-Regeln werden mit MSTest abgedeckt; Client- und Browserflüsse erhalten automatisierte E2E-Checks sowie manuelle Hardwareabnahme.
- Kein Akzeptanzwert gilt als erfüllt, bevor er in einer reproduzierbaren Konfiguration gemessen wurde.

| Bereich | Prüffall / Akzeptanzkriterium |
|-|-|
| Rundenlänge | Standardrunde endet serverautoritativ nach $60\text{ s}$; konfigurierbarer Bereich lässt nur $30$ bis $90\text{ s}$ zu. |
| Touch | Tap und Swipe erzeugen lokal sichtbares Feedback in $\leq 50\text{ ms}$ p95 auf dem Zielhandheld. |
| Framerate | Aktiver Client erreicht $\geq 55\text{ FPS}$ p95, Watcher $\geq 50\text{ FPS}$ p95 unter dem festgelegten Maximalspawnprofil. |
| Hintergrund und Verdeckung | Jedes Ziel ist mindestens $650\text{ ms}$ vor der ersten Verdeckung vollständig sichtbar; keine Verdeckung überschreitet $40\,\%$ Trefferfläche oder $450\text{ ms}$. |
| Retro-Editor-Huelle | Die gelbe Arbeitsflaeche ist der einzige Ziel- und Gestenbereich; linke Achsen reagieren sichtbar auf Laser und Schleifswipe, ohne selbst Eingabe zu werden. |
| Funktions- und F-Tastenleiste | `F1` ist ein nicht rastender Starttaster. `F2` bis `F6` sind ausreichend grosse, visuell rastende Schalter; `F2 LASER` und `F3 SCHEIBE` bleiben stets gegenseitig ausschliessend, genau einer ist aktiv. Die grauen Tasten enthalten ausser `F1` bis `F6` keinen weiteren Text. Alle rechten Ereignistasten sind wertungsneutral und respektieren `TON`. |
| Laser- und Schleifscheiben-Inszenierung | Die linke Maschine zeigt Portaltraeger, Schlitten, Kabelschlauch, Fokuskopf sowie eine seitliche Scheibe mit Schutzring. Ein Swipe entlang der sichtbaren Schneide zeigt gerichtete Achsbewegung, Scheibenanlauf und Funkenstreifen, ohne eine freie Maschinensteuerung oder unklare Trefferzone zu erzeugen. |
| Kleine Viewports | Im schmalen Hochformat bleibt die gelbe Spielflaeche mindestens $70\,%$ breit; die Ereignisleiste wechselt ohne Ueberlappung in eine Symbolschublade. |
| Highscore und Neustart | Name oder Fantasiename plus Ranglisteneinwilligung genügen, E-Mail darf leer bleiben; Überspringen/Timeout gibt ohne Speicherung frei. `LASER` und `SCHEIBE` bleiben getrennt. Gültige Einträge überstehen einen Neustart; gelöschte/abgelaufene nicht. E-Mail fehlt in öffentlichen DTOs, Logs und Kabelanzeige. |
| Leads und Preisvergabe | E-Mail-Feld ist aktiv; leerer Wert verhindert nur Gewinnspiel-/E-Mail-Kontaktteilnahme, nicht Spiel/Highscore. Ungültige ausgefüllte Adresse wird beanstandet. Preisberechtigte Exportliste enthält ausschließlich Teilnehmer mit E-Mail und bestätigten Bedingungen. Vertriebsansprache nur bei separater Einwilligung; deren Ablehnung verändert die Gewinnchance nicht. Daten sind nach Messeende noch zur Preisabwicklung vorhanden; Export nur für Betreiber. |
| Synchronisierung | Befehl bis Server p95 $\leq 75\text{ ms}$, bestätigter Clientzustand p95 $\leq 150\text{ ms}$ lokal; optionaler Netzwerk-Watcher p95 $\leq 250\text{ ms}$ im gewählten Netz. Funkverlust beeinflusst lokale Wertung und Kabelanzeige nicht. |
| Rollen | Zwei parallele Beitrittsversuche ergeben stets genau eine aktive Rolle; nur der Betreiber-Slide kann freigeben oder abbrechen, und jeder Wechsel ist atomar und versioniert. |
| Paketverlust | Wiederholte Nachricht und Versionslücke ändern Punktestand/Ereignis nie doppelt; Snapshot stellt Konsistenz wieder her. |
| Offline und Kabel | Kaltstart, Runde, Rangliste, lokale Watcher-Ansicht, Musik und Assets funktionieren mit deaktiviertem WLAN und gezogenem Netzwerkkabel. Externen Bildschirm vor/während/nach der Runde anschließen und abziehen: keine zweite aktive Rolle, kein Timerreset, keine Veröffentlichung privater Formulare. Audioausgang nach Hotplug prüfen. |
| Full HD und Skalierung | 1920 × 1080 bei 100/125/150 % Windows-Skalierung; UI, Canvas und Trefferzonen stimmen überein. Andere Seitenverhältnisse verzerren keine Zonen, Mindesttouchflächen und Letterboxing bleiben erhalten. |
| Wertung | Erfolg +10, falsches Schärfen -5, Schaft -10, Fehlschlag -5 als Startprofil; geänderte Werte ohne Build aktivierbar. Zonengrenzen, PKD-/Nicht-PKD- und Größenvarianten prüfen; ein Versuch zählt einmal, negative Stände enden erst mit dem Timer. |
| Audioereignisse und Pools | Je Modus nur `success` und `fail`; Schaft/Fehlbearbeitung/Fehlschlag teilen den Fail-Pool. Jedes bestätigte Ereignis startet Kurzfeedback. Poolgrößen 1, 3, 6 und 8 ohne Codeänderung prüfen, leerer Pool nutzt Fallback. Alle Varianten vor erneutem Mischen einmal ziehen; ab zwei Varianten keine direkte Wiederholung. Wiederholte Event-ID, Snapshot und Reconnect sind stumm. |
| Audio-Burst und Ducking | Zehn Aktionen in einer Sekunde: höchstens zwei Effektquellen plus Musik, keine wachsende Queue, veraltete Stimmen nach spätestens 200 ms verwerfen. Musikabsenkung 12 dB, Attack 20 ms, Hold 120 ms, Release 250 ms; keine kumulative Absenkung, kein unbeabsichtigtes Entstummen. Kurzfeedback p95 höchstens 100 ms nach Gestenende. |
| Audiofehler und Ausgabe | Fehlende/defekte Sprachdatei erhält Kurzfeedback, fehlendes Kurzfeedback nutzt Ersatzton; keine Musik blockiert keine Runde. Autoplay-Sperre, F6 laut/leise/stumm und Wechsel des Windows-Audioziels prüfen; beide Watcherarten bleiben stumm. Finale Dateien anhören und Pegel/Clipausschnitte prüfen. |
| Datenschutz-Overlay | Kreis-i vor Dateneingabe erreichbar; langer Hinweis vollständig mit Touch/Maus/Tastatur scrollbar, Schließen bei 200 % Zoom und Bildschirmtastatur sichtbar. Fokusführung, Escape, keine Canvas-Durchgriffe, keine Timeout-Löschung beim aktiven Lesen. |
| Datenschutz-Konfiguration und Löschung | Textwechsel ohne Neubau ab nächstem Formular, alte Version für offene Formulare; ungültige Fassung aktiviert keine Speicherung. Ranglisten-, Gewinnspiel- und Vertriebszwecke getrennt validieren. Widerruf der Vertriebsansprache lässt unabhängig gewünschte Gewinnspielteilnahme bestehen. Löschcode, zweckbezogene Fristen nach Preisabwicklung, Exporte und Sicherungswiederherstellung prüfen. |
| Asset-Austausch | Ein validiertes Manifest nimmt ein neues Asset mit nächster freier Kategorie-ID und passender Vorlage vor einer Runde ohne Kernlogikänderung auf; doppelte, fehlende oder vorlagenwidrige IDs blockieren die Aktivierung. |
| Bild-/Audio-Mapping | Ein neues Hintergrund-, Humor- oder Ereignisasset referenziert seine optionale Audioereignisgruppe allein im Manifest; bei fehlendem Audio bleibt das Bildereignis ohne Unterbrechung sichtbar. |
| Fallback | Watcher-Neustart beeinflusst die Runde nicht; Handheld-Reconnect folgt der definierten Frist; Serverausfall führt zu kontrolliertem Neustart oder getrenntem Demo-Modus. |

Unit-Tests sichern Punkte-, Combo-, Zeit-, Rollen- und Deduplizierungsregeln ab. Integrationstests prüfen Hub-/Snapshotverträge, Konkurrenz und Reconnect. E2E-Tests decken Start, Gesten, Runde, Ergebnis und Watcherbeitritt ab. Zusätzlich sind ein Dauerlauf über mindestens vier Stunden, ein Paketverlusttest, ein Kaltstarttest und eine Vor-Ort-Abnahme mit finalen Geräten verpflichtend.

# 7. Risiken

| Risiko | Kurzbeschreibung | Risikohöhe |
|-|-|-|
| R-01 | Ungeprüfte Touch-, Monitor- oder Audioanschlüsse gefährden den Kabelbetrieb. | hoch |
| R-02 | Selbst ergänzte Fremdmedien könnten von der geklärten royaltyfree-/Eigenkreationsvorgabe abweichen. | mittel |
| R-03 | Hohe Partikeldichte oder ungetestete Browserkonfiguration senkt die Bildrate. | mittel |
| R-04 | Serverausfall unterbricht die gemeinsame Wertungsrunde. | mittel |
| R-05 | Überlappende Stimmen, Autoplay-Sperren oder Audio-Umschaltung zerstören das hörbare Feedback. | mittel |
| R-06 | Gewinnspieladressen könnten ohne separate Einwilligung für Vertriebsansprache verwendet oder vor Preisabwicklung gelöscht werden. | hoch |

## 7.1 R-01: Messehardware und Kabelbetrieb
### tl;dr
- **Favorit:** Original-Convertible, Monitor, Adapter und Audioausgang gemeinsam ohne Netz testen.

### Detail
Der aktive Spielpfad hängt nicht am WLAN. Touchpanel, DPI-Skalierung, Anschlussadapter und Windows-Audioumschaltung bleiben jedoch konkrete Ausfallquellen. Zwei lokale Browseransichten belasten dieselbe GPU.

### Empfehlung
- **Favorit:** Windows 11, erweiterten Desktop, festen Audioausgang und geprüften Ersatzadapter verwenden; Kaltstart und Hotplug in AP-06 nachweisen.
- **Begründung:** Genau diese Kombination sichert den verbindlichen Kabel-Fallback unabhängig vom Messefunk.

## 7.2 R-02: Medienrechte ohne Freigabeprozess
### tl;dr
- **Favorit:** Die bestätigte royaltyfree-/Eigenkreationsvorgabe auch auf selbst ergänzte Assets und Schriften anwenden.

### Detail
Der Nutzer pitcht und bearbeitet selbst und verwendet ausschließlich royaltyfree Quellen oder eigene Kreationen/Kompositionen. Diese Grundlage ist geklärt; es besteht keine offene Rechtefrage zur Konzeptentscheidung. Bei neuen Fremdassets gelten deren konkrete Nutzungsbedingungen auch dann, wenn sie als royaltyfree angeboten werden.

### Empfehlung
- **Favorit:** Nutzervorgaben übernehmen; für eigene Ergänzungen frei verwendbare Gestaltung und Schriften wählen, nötige Lizenzhinweise mitliefern.
- **Begründung:** Das hält die geklärte Mediengrundlage ein, ohne weitere Freigaberunde oder Beschaffungspflicht.

## 7.3 R-03: Performance der Effekte
### tl;dr
- **Favorit:** Partikel- und Spawnobergrenzen profilbasiert konfigurieren und auf Finalhardware testen.

### Detail
Laser, Funken und große Werkzeugtexturen können mobile GPUs überlasten.

### Empfehlung
- **Favorit:** Qualitätsprofile mit festen Obergrenzen, Object Pooling und Performancebudget je Zielgerät.
- **Begründung:** Visuelle Qualität bleibt steuerbar, ohne die Spielregeln oder Assets zu duplizieren.

## 7.4 R-04: Ausfall des autoritativen Servers
### tl;dr
- **Favorit:** Kontrollierter Neustart mit vorbereitetem Ersatzhost statt komplexem Hochverfügbarkeitscluster.

### Detail
Ohne Server ist eine gemeinsame wertungsrelevante Runde nicht zuverlässig fortsetzbar.

### Empfehlung
- **Favorit:** Lokale, atomare Zustandsablage nur für Diagnose/Restart, ein getesteter Ersatzhost und klarer Neustartfluss.
- **Begründung:** Das reduziert Messekomplexität und ist für eine zeitlich begrenzte Installation zuverlässiger als Live-Failover.

## 7.5 R-05: Audioüberlastung und Stille
### tl;dr
- **Favorit:** Einen gemeinsamen Mixer mit Kurzfeedback, einer Stimme und einem Musikstream einsetzen.

### Detail
Schnelle Eingaben können Stimmen stapeln; Browser-Autoplay und HDMI-Hotplug können einen vorhandenen Audiopfad stummschalten oder umlenken.

### Empfehlung
- **Favorit:** Limits und Ducking aus Kap. 3.4.3 verbindlich implementieren, Watcher stummschalten und einen lokalen Tontest anbieten.
- **Begründung:** Das kurze Aktionsfeedback bleibt erhalten, während Stimmen und Musik kontrollierbar bleiben.

## 7.6 R-06: Datenschutz im tatsächlichen Betrieb
### tl;dr
- **Favorit:** Gewinnabwicklung und weitergehende Vertriebsansprache mit getrennten Zwecken und Aufbewahrungsfristen umsetzen.

### Detail
Lead-Erzeugung und Preisbenachrichtigung nach der Messe sind festgelegt. Eine reine Gewinnspieladresse darf nicht automatisch als allgemeine Werbeeinwilligung behandelt werden; vorzeitige Löschung würde die nachgelagerte Preisvergabe verhindern.

### Empfehlung
- **Favorit:** Den Disclaimer aus Kap. 3.8 einsetzen, Preisvergabe an erreichbare E-Mail binden, Vertriebsansprache separat erlauben lassen und Daten erst nach zweckbezogenen Fristen löschen.
- **Begründung:** Das unterstützt Leads und Nachmesse-Kontakt, ohne Fantasienamen-Spieler auszuschließen oder eine unbegrenzte Werbeerlaubnis zu unterstellen.

# 8. Offene Punkte

| Offener Punkt | Kurzbeschreibung | Status |
|-|-|-|
| OP-01 | Windows-Convertible und Full-HD-Annahme festgelegt; Modell, Auflösung und Anschlüsse fehlen | ❌ zu klären |
| OP-02 | Lokaler Betrieb und Kabel-Fallback; Funk nur als Option | ✅ geklärt |
| OP-03 | Eigene/royaltyfree Medien; ohne Nutzervorgabe frei verwendbare Farben und Schriften | ✅ geklärt |
| OP-04 | Leads, Fantasienamen und Preisbenachrichtigung per E-Mail nach der Messe; konfigurierbarer DSGVO-Hinweis | ✅ geklärt |
| OP-05 | Fach-/Marketingfreigabe entfällt ausdrücklich | ✅ geklärt |
| OP-06 | Bestehender Stack auf einem Windows-Gerät | ✅ geklärt |
| OP-07 | Säge-, UI- und Musiklieferung sowie Hörzuordnung der Laserdateien | ❌ zu klären |

## 8.1 ❌ OP-01: Zielhardware und Aufstellungsdetails
### tl;dr
- Festgelegt: Windows-Convertible, bevorzugt Windows 11, Server und aktiver Client auf demselben Gerät; Planungsformat Full HD quer.
- Fehlend: Modell, endgültiges OS, native Auflösungen, DPI-Skalierung, Monitor-/Adapteranschluss und Audioausgang.

### Detail
Der Nutzer liefert die Auflösung nach. Ein separates Handheld oder ein externer Server ist nicht erforderlich. Windows 10 wird nur bei tatsächlicher Gerätewahl samt passender Laufzeit-/Browserunterstützung geprüft.

### Empfehlung
- **Favorit:** Bis zur Lieferung mit 1920 × 1080 und Windows 11 planen; Hardwaredaten in AP-01 nachtragen, Originalaufbau in AP-06 prüfen.
- **Begründung:** Das Projektgerüst kann beginnen, ohne eine noch unbestätigte Hardwareabnahme vorzutäuschen.
- **Nächste Aktion:** Gerätemodell und Anschlussdaten vom tatsächlichen Convertible/Monitor erfassen; Kabel und Adapter für den Netz-aus-Test bereitstellen.

## 8.2 ✅ OP-02: Lokaler Betrieb statt WLAN-Abhängigkeit
### tl;dr
- Geklärt: Server und Client lokal, Kabelanzeige verbindlich, Beobachterclient optional.

### Detail
Der Messefunk liegt außerhalb des kritischen Spielpfads. Ein drahtloser Watcher darf genutzt werden, wenn vor Ort eine geeignete Verbindung verfügbar ist; ohne diese bleibt die Kabelanzeige vollständig nutzbar.

### Empfehlung
- **Favorit:** Loopback als Standard, lokale passive Zweitanzeige über Monitorkabel; externen Watcherzugang nur bewusst aktivieren.
- **Begründung:** Das erfüllt Offlinebetrieb und spontane Funkoption ohne Netzabhängigkeit oder zusätzliche aktive Rolle.
- **Nächste Aktion:** AP-02 paketiert lokal; AP-06 prüft Funkverlust und Kabel-Hotplug.

## 8.3 ✅ OP-03: Medien und Gestaltung geklärt
### tl;dr
- Der Nutzer pitcht/verändert selbst und verwendet ausschließlich royaltyfree Quellen oder eigene Kreationen/Kompositionen.
- Ohne Nutzervorgabe zu Farbe und Schrift wird frei verwendbare Gestaltung gewählt.

### Detail
Die Mediengrundlage ist durch die Nutzerangabe geklärt. Es gibt keinen zusätzlichen kreativen Freigabeprozess und keine Pflicht zur Verwendung einer kostenpflichtigen Corporate-Schrift.

### Empfehlung
- **Favorit:** Gelieferte Eigen-/royaltyfree Medien übernehmen und notwendige eigene Ergänzungen einschließlich Schriften frei verwendbar auswählen.
- **Begründung:** Das setzt die klare Nutzervorgabe ohne zusätzliche Klärungsschleife um.
- **Nächste Aktion:** Reguläre Assetübernahme in AP-05; vorhandene Lizenzbedingungen/-hinweise bei der Paketierung berücksichtigen. Keine offene Konzeptentscheidung.

## 8.4 ✅ OP-04: Leads und Preisbenachrichtigung geklärt
### tl;dr
- Ziel: Leads erzeugen und mit potenziellen Kunden ins Gespräch kommen; Name und E-Mail erfassen.
- Ohne Kontaktdaten genügt ein Fantasiename; einen Preis nach der Messe gibt es nur mit erreichbarer E-Mail-Adresse.

### Detail
Der Verarbeitungszweck ist festgelegt, das E-Mail-Feld wird nicht deaktiviert. Kap. 3.8 enthält den DSGVO-Hinweis für Rangliste, Gewinnspiel und erlaubte Vertriebsansprache. Kreis-i, scrollbar bedienbares Overlay und Textänderung ohne Neubau bleiben verbindlich.

### Empfehlung
- **Favorit:** Name/Fantasiename und E-Mail im Ergebnisformular anbieten; Preisabwicklung an E-Mail, weitergehende Vertriebsansprache an separate freiwillige Einwilligung binden.
- **Begründung:** Unterstützt den Lead-Zweck und die Preisvergabe nach Messeende, während Spielen ohne Kontaktdaten möglich bleibt.
- **Nächste Aktion:** Formular und Hinweis in AP-05 umsetzen, Betreiberangaben und Termine bei der Einrichtung konfigurieren. Keine offene Zweck- oder Konzeptentscheidung; keine Behauptung einer bereits abgeschlossenen Implementierung oder rechtlichen Zertifizierung.

## 8.5 ✅ OP-05: Entfällt, bewusstes Überraschungsprojekt
### tl;dr
- Die bisherige Fach-/Marketingfreigabe entfällt ausdrücklich auf Nutzerentscheidung.

### Detail
Das Spiel ist ein „U-Boot“ und eine Überraschung. Retro-Romantik, Selbstironie und offensiver Humor sind gewollt, nicht bis zu einem Workshop vertagt. PKD-Bohrer/-Fräser mit Schaft sowie Sägen und Punkteabzüge sind als konkrete Spielidee beschrieben, nicht als Maschinensimulation.

### Empfehlung
- **Favorit:** Kein Workshop, keine zusätzliche Freigaberolle und kein OP-05-Gate im Umsetzungsplan.
- **Begründung:** Das entspricht ausdrücklich Ziel, Tonalität und Überraschungscharakter.
- **Nächste Aktion:** Keine Klärung erforderlich; technische Spielbarkeit wird regulär getestet.

## 8.6 ✅ OP-06: Technologieempfehlung
### tl;dr
- Die Entscheidung ist als Konzeptempfehlung getroffen, ihre Realisierungsdetails werden in AP-02 validiert.

### Detail
Blazor Server ist kein geeigneter primärer Echtzeit-Client; reines Phaser löst den lokalen Messebetrieb weniger vollständig. Option C trennt diese Verantwortungen.

### Empfehlung
- **Favorit:** Lokaler ASP.NET-Core-Server, Phaser/TypeScript-Client und Blazor-Shell auf demselben Windows-Convertible; Watcher lokal oder optional extern.
- **Begründung:** Direkte Touch-Interaktion, keine WLAN-Abhängigkeit und eine wiederverwendbare passive Beobachteransicht.
- **Nächste Aktion:** AP-02 validiert Start, Paketierung und Kioskmodus auf Zielhardware.

## 8.7 ❌ OP-07: Audiolieferung und Hörzuordnung
### tl;dr
- Vorhanden: zehn Laser-MP3s unter bisherigen `yell`-Namen. Startvorschlag: Übernahme unter Aktionsnamen und 22 zusätzliche Dateien gemäß Kap. 3.4.4; 3–6 Varianten pro Aktion sind Ziel, keine technische Grenze.
- Fehlend: Säge-Stimmen, kurze Feedback-/UI-Sounds, Arcade-Musik und Hörprüfung der vorhandenen Laserdateien.

### Detail
Alle Aktionen und Zufallspools sind mit konkreten Dateinamen spezifiziert. Die Planung stützt sich auf den Dateibestand, nicht auf eine hier erfolgte Klang- oder Längenanalyse.

### Empfehlung
- **Favorit:** Genau die Lieferliste aus Kap. 3.4.4 verwenden; Zuordnung nach Hörcheck gegebenenfalls allein im Manifest korrigieren.
- **Begründung:** Der Nutzer weiß vorab, welche Dateien fehlen, und die Umsetzung braucht keine zusätzlichen unbenannten Soundpakete.
- **Nächste Aktion:** Laserbestand auf `laser_success_*`/`laser_fail_*` verteilen; jeweils drei `saw_success_*`, `saw_fail_*`, `feedback_success_*`, `feedback_fail_*`, `ui_confirm_*`, `round_start_*`, `round_end_*` sowie `arcade_music_01.mp3` liefern oder produzieren. Exakte Namen und Auslöser stehen in Kap. 3.4.4; AP-06 prüft Klang, Pegel, Loopgrenzen und Mischer.
