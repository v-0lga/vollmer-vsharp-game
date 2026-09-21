# Vollmer Messe-Game
| | |
|-|-|
| **Erstelldatum:** | 2026-09-09 |
| **Letzte Änderung:** | 2026-09-15 |
| **Issue:** | 00001 |

## Commit-Vorschlag für den finalen Gesamt-Commit

> **Status:** `vorläufig` — Der Vorschlag wird nach Umsetzung, Einarbeitung aller Code-Review-Findings und erfolgreicher finaler Review anhand der tatsächlich enthaltenen Änderungen aktualisiert. Es wird kein separater Konzept-Commit erstellt.

```text
[00001] Interaktives Vollmer Messe-Game bereitstellen

- Geplante Umsetzung: lokales Touch-Arcade-Spiel mit aktivem Handheld und passiver Zuschaueranzeige.
- Geplante Entscheidung: autoritativer ASP.NET-Core-Server, Phaser/TypeScript-Spielclients und Blazor-Betriebshülle.
- Geplante Absicherung: automatisierte Regeltests sowie Touch-, Latenz-, Last- und Hardware-Abnahme.
- Offene Punkte: Zielhardware, Corporate Assets, Datenschutz und verbindliche Messeanforderungen.
```

# 0. Entscheidungsvorlage

| | |
|-|-|
| **Problem** | Ein Messe-Eye-Catcher soll Werkzeugscharfen intuitiv, zuverlässig und ohne Internet auf Handheld und Watcher vermitteln. |
| **Lösung** | Phaser/TypeScript rendert den lokalen Touch-Game-Loop; ein lokaler ASP.NET-Core-Server verwaltet Runden und genau eine aktive Rolle, Blazor übernimmt den Betrieb. |
| **Entscheidungsbedarf** | Zielhardware, exklusiver Access Point, Corporate-/Produktassets, Namens- und Highscore-Regeln sowie Lizenzfreigaben bestätigen. |
| **Top-Risiko** | Ungetestetes Messe-WLAN oder ungeeignete Touch-Hardware kann die wahrgenommene Reaktion und Synchronität beeinträchtigen. |
| **Blockierende offene Punkte** | OP-01 Hardware, OP-02 Netzwerk, OP-03 Marken-/Assetrechte, OP-04 Datenverarbeitung und OP-05 Freigabe der Spielregeln. |

# 1. Kontext, Zielsetzung & Use Case

## 1.1 tl;dr
- Das Spiel soll binnen weniger Sekunden zum Mitmachen motivieren und Vollmers Kompetenz bei Schleifen, Schärfen, Erodieren und Laserbearbeitung verständlich machen.
- Eine Runde dauert standardmäßig $60\text{ s}$; $30$ bis $90\text{ s}$ bleiben als konfigurierter, abnahmefähiger Rahmen zulässig.
- Ein Handheld ist der einzige aktive Touch-Eingabepunkt; der große Bildschirm zeigt dieselbe Runde passiv für Zuschauer.

## 1.2 Ziele und Nicht-Ziele

| Ziel | Beschreibung |
|-|-|
| Z-01 | Messebesucher erkennen ohne Einweisung den Unterschied zwischen präzisem Laser-Tap und kraftvollem Schleif-Swipe. |
| Z-02 | Eine vollständige, wertungsrelevante Runde ist lokal und ohne Internet spielbar. |
| Z-03 | Werkzeuge, Effekte, Texte, Schwellenwerte und Audio sind über versionierte Konfiguration und Assets austauschbar. |
| Z-04 | Der Watcher verstärkt das Geschehen, ohne eine zweite Eingabequelle oder einen zweiten Spieler zu erzeugen. |
| Z-05 | Der Aufbau ist vom Standpersonal mit einem definierten Startablauf und Fallback bedienbar. |

| Nicht-Ziel (Scope) | Begründung |
|-|-|
| NZ-01 | Keine realistische CNC-/Maschinensimulation. | Die kurze Messeinteraktion priorisiert Verständlichkeit und Reaktionsfreude. |
| NZ-02 | Keine cloudabhängigen Konten, Online-Bestenlisten oder Telemetrie. | Der Messebetrieb muss ohne Internet auskommen und Datenschutzaufwand klein halten. |
| NZ-03 | Kein gleichzeitiges Mehrspieler-Spiel. | Genau eine aktive Rolle verhindert widersprüchliche Touch-Eingaben und vereinfacht den Betrieb. |
| NZ-04 | Keine produktive Asset-Erstellung in diesem Konzept. | Es werden Struktur und Gestaltungsrichtung beschrieben; Freigabeassets fehlen noch. |

## 1.3 Zielgruppen- und Messekontext

Primäre Nutzer sind vorbeigehende Fachbesucher und Begleitpersonen mit unterschiedlichen Vorkenntnissen. Die Interaktion muss sowohl im Vorbeigehen lesbar als auch ohne Erklärung spielbar sein. Sekundäre Nutzer sind Zuschauer am großen Bildschirm und das Standpersonal, das Startbereitschaft, Lautstärke und Störungen steuert.

Der Ablauf beginnt im Attract-Mode: bewegte Werkzeugsilhouetten, dezente Maschinenklänge und eine kurze, rein visuelle Demonstration von Tap und Swipe ziehen Aufmerksamkeit an. Ein großer Startbereich beginnt die Runde. Eine kurze Vorführung während der ersten Ziele ersetzt eine Textanleitung. Nach dem Ergebnis kehrt das System automatisch zum Attract-Mode zurück.

# 2. Analyse / Ist-Zustand

## 2.1 tl;dr
- Das Repository enthält nur [README.md](../../README.md), [LICENSE](../../LICENSE), [.gitignore](../../.gitignore), Editorfarben und GitHub-Copilot-Arbeitsvorgaben; es gibt weder Anwendung noch Assets noch Buildkonfiguration.
- Die Dokumentationsstruktur war leer; [00000-issues.md](../00000-issues.md), dieses Konzept und die Fortschrittsseite bilden den ersten dokumentierten Arbeitsstand.
- C# wird durch vorhandene Rollen und den MSTest-Skill als naheliegende Teamoption gestützt, ist aber noch keine bestätigte Technologieentscheidung.

## 2.2 Repository-Vorgaben und Lücken

[.github/copilot-instructions.md](../../.github/copilot-instructions.md) verlangt deutsche Dokumentation und Kommentare, transparente Angaben zu fehlenden Punkten, konkrete Empfehlungen für Entscheidungen/Risiken und enge, nachweisbare Validierung. Der Skill [concept-plan](../../.github/skills/concept-plan/SKILL.md) schreibt die Kapitel 0 bis 8, den Index und die Fortschrittsseite vor; [implementation-workflow](../../.github/skills/implementation-workflow/SKILL.md) erklärt diese Fortschrittsseite für die spätere Umsetzung zur verbindlichen Arbeitsliste. Bei C#-Tests gilt [csharp-mstest](../../.github/skills/csharp-mstest/SKILL.md).

Es existieren keine Projektdateien, Quelltexte, Tests, CI-Workflows, Build-/Deployment-Skripte, Prototypen oder Medien. Die globale [.gitignore](../../.gitignore) ist laut Kopfkommentar verbindlich und soll nicht projektspezifisch verändert werden; ein späterer `Assets/`-Ordner ist jedoch vorgesehen und nicht ausgeschlossen. Die MIT-Lizenz nennt `v-0lga`, während README und Gitignore Vollmer-Kontext tragen. Daraus folgt OP-03: Rechte und Freigabe von Logo, Produkten, Medien und Vertrieb müssen vor Veröffentlichung geklärt werden.

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
4. **Feedback:** Richtige Aktionen zeigen sofort Laserblitz beziehungsweise Schleifkontakt, Funken, Schärfeindikator, Qualität, Punkte und Combo. Fehlaktionen brechen die Combo ab oder kosten eine konfigurierbare geringe Punktzahl; sie führen nicht zu einem abrupten Rundenaus.
5. **Ende:** Der Server beendet nach $60\text{ s}$ die Wertung, beide Ansichten zeigen Ergebnis und die Rangliste des gespielten Modus. Der Name ist ein Pflichtfeld mit konfigurierbarer Maximallaenge und wird ueber die Bildschirmtastatur eingegeben; eine E-Mail-Adresse ist optional. Die Eingabe dient nur dem Highscore, Leads entstehen ausschliesslich im persoenlichen Messegespraech.
6. **Rückkehr:** Nach konfigurierbaren $10$ bis $20\text{ s}$ ohne Eingabe oder nach Abschluss der Namenseingabe wird die Rolle freigegeben und der Attract-Mode geladen.

**Rollenwechsel:** Die reguläre Spieleransicht erhält kein frei sichtbares Rollenmenü. Am Watcher beziehungsweise Betreiber-Host öffnet ein unauffälliger, dokumentierter Langdruck einen Betreiber-Dialog. Der Dialog zeigt den verbundenen aktiven Client und bietet die Aktionen `Rolle freigeben` sowie `Runde abbrechen und Rollen freigeben`. Beide Aktionen verlangen einen bewusst ausgeführten Slide-to-confirm-Schalter. Während einer laufenden Runde ist nur die zweite Aktion verfügbar, damit eine Wertung nicht still an eine andere Person übergeht. Nach einer Freigabe oder einem Abbruch kehren alle Clients zum Attract-Mode zurück; der nächste Handheld-Tap kann die aktive Rolle übernehmen. Der Server führt die bereits definierte atomare Übergabe mit erhöhter `RoleVersion` aus.

**Laser-Modus:** PDC-bestückte Präzisionswerkzeuge und feine Bohrer besitzen schlanke Silhouetten, cyanfarbene Zielmarkierungen und kurze Präzisionsfenster. Ein Tap löst einen punktförmigen Lichtimpuls, einen klaren Hochton und einen sichtbaren Schärfegrad aus.

**Schleif-Modus:** Massive HM-Fräser und Sägeblätter erscheinen größer mit warmen Kontaktzonen. Ein Swipe entlang der markierten Schneide löst eine virtuelle Schleifscheibe, gerichtete Funken und einen tieferen Kontaktklang aus. Richtung und Mindeststrecke sind sichtbar, aber tolerant konfigurierbar.

## 3.4 Grafik-, Asset- und Soundkonzept

Die visuelle Richtung verbindet eine helle, präzise Industrieoberfläche mit dunklem Maschinenraum: neutrale Graphit-/Stahltöne für Hintergrund und Werkzeuge, Vollmer-Farben ausschließlich aus freigegebenem Corporate Design für relevante Akzente. Werkzeuge benötigen klar lesbare Silhouetten und die Zustände `dull`, `in_progress` und `sharp`; fotorealistische Details sind nur dort sinnvoll, wo sie die Form nicht verschleiern.

Das HUD bleibt am Handheld kompakt: oben Zeit und Punkte, darunter Combo und Modusindikator. Der Watcher nutzt dieselben Ereignisse, zeigt aber großformatige Werkzeugbewegungen, den aktuellen Spielerstatus, Punkte, Combo und optional die Tagesrangliste. Der Watcher enthält keine Eingabecontrols.

Empfohlene Struktur: `Assets/images`, `Assets/animations`, `Assets/audio`, `Assets/fonts`, `Assets/templates` und `Assets/config`. Jedes Asset liegt in genau einer Kategorie und verwendet eine monotone, pro Kategorie eindeutige ID statt eines fachlichen Namens: `Assets/images/tools/tool_0001.png`, `Assets/animations/laser/fx_0001.json`, `Assets/audio/grind/sfx_0001.ogg`. Die ID ist mindestens vierstellig und wird nie wiederverwendet; eine entfernte Datei hinterlässt ihre ID als reserviert. Damit sind beliebig viele Assets ohne Umbau der Kernlogik ergänzbar, solange sie einer vorhandenen Kategorie und deren Vorlage entsprechen.

Die fachliche Bedeutung liegt nicht im Dateinamen, sondern ausschließlich im versionierten Manifest, beispielsweise `tool_0001.png` mit `toolType: "fine_drill"`, `interaction: "tap"`, `states: ["dull", "sharp"]` und zugeordneten Effekt-/Audiokategorien. Eine Assetvorlage pro Kategorie dokumentiert Pflichtfelder, Formate, Pivot, Auflösung, zulässige Varianten, Performancegrenzen und Fallbacks. Ein Manifest-Validator prüft vor dem Start jeder Runde IDs, Kategorien, referenzierte Dateien und Vorlagenkonformität; bei Fehlern lädt er das letzte gültige Manifest und protokolliert die Abweichung. PNG/WebP eignet sich für 2D-Rastergrafiken, JSON-Atlanten für Sprites, OGG für Effekte/Musik und WAV nur für Quellmaterial oder kritische kurze Effekte nach Hardwaretest.

| Dateiname | Zweck und Anzeigeort | Format und technische Anforderungen | Austauschbar über Konfiguration | Erwartete Varianten |
|-|-|-|-|-|
| `images/tools/tool_0001.png` | Werkzeugbild in Handheld und Watcher | PNG/WebP mit Transparenz, Quelle mindestens 1024 px längste Kante, Vorlage `tool-image` | ja | `dull`, `sharp`, Werkzeugtyp |
| `animations/laser/fx_0001.json` | Laserblitz | Phaser-kompatibler Partikel-/Sprite-Atlas, Vorlage `laser-effect` | ja | Intensität, Farbe, Dauer |
| `animations/grind/fx_0001.json` | Schleiffunken | Atlas/JSON mit begrenzter Partikelzahl, Vorlage `grind-effect` | ja | Richtung, Dichte, Material |
| `images/ui/attract_0001.webm` | Attract-Hintergrund am Watcher | lokales WebM, stumm oder mit separatem Audio, Vorlage `watcher-video` | ja | Hochformat, Querformat |
| `audio/laser/sfx_0001.ogg` | bestätigter Lasertreffer | OGG, mono/stereo nach Test, normalisiert, Vorlage `laser-sfx` | ja | gewichtete Treffergruppe |
| `audio/grind/sfx_0001.ogg` | bestätigter Schleifkontakt | OGG, kurz und loopfrei, Vorlage `grind-sfx` | ja | gewichtete Kontaktgruppe |
| `audio/music/music_0001.ogg` | dezente Standby-Musik | OGG mit Loopdaten im Manifest, Vorlage `music-loop` | ja | alternierende Stücke |
| `audio/ui/sfx_0001.ogg` | Rundenende | OGG, kurz, klarer Ausklang, Vorlage `ui-sfx` | ja | Standard, Highscore |

Audio ist in `Assets/audio/{ui,laser,grind,ambient,music}` organisiert. Ein Audioprofil legt getrennte Lautstärken für Musik und Effekte fest, wählt gewichtete Varianten und definiert eine stumme Ersatzreaktion bei fehlender Datei. Audio darf eine sichtbare Aktion nie blockieren. Nur freigegebene oder selbst produzierte Medien gehen in ein Offline-Release; die Rechtekette wird im Asset-Manifest dokumentiert.

Ein lizenzfreies Anschauungspaket für alle acht Kategorien liegt unter [demo-material](../demo-material/README.md). Es dient ausschließlich der Abstimmung; erst die freigegebenen Produktionsassets und ihr validiertes Manifest sind Teil des Releases. **Die technische und gestalterische Qualität dieses Anschauungsmaterials ist ausdrücklich kein Maßstab für die Produktionsqualität des Games.** Das Paket beweist nur Kategorien, Dateiablage, ID-Vergabe und Manifest-Referenzen. Die Produktionsabnahme verlangt separat freigegebene Marken-/Produktassets, Art Direction, Qualitätsprofile, Performancebudget und Tests auf Finalhardware.

## 3.5 Hintergrund, räumliche Tiefe und Humor

**Favorit:** Eine stilisierte, ruhig automatisch scrollende Produktionslandschaft mit drei Parallaxebenen. Das erzeugt die räumliche Spannung bekannter Arcade-Sidescroller, ohne eine hektische Moorhuhn-Kopie oder eine technische Maschinensimulation zu werden. Die Welt bewegt sich horizontal, die Kamera bleibt ruhig; Besucher müssen weder scrollen noch eine Kamera steuern.

| Ebene | Parallaxfaktor | Inhalt | Spielregel |
|-|-:|-|-|
| Hintergrund | $0{,}15$ bis $0{,}25$ | Hallenstruktur, entfernte Maschinen, Lichtbaender und abstrakte Markenflaechen nach Freigabe | rein dekorativ, nie Zielverdeckung |
| Mittelebene | $0{,}55$ bis $0{,}70$ | Werkzeuge, Zufuehrung, Bearbeitungszellen und Qualitaetsstationen | alle Ziele und Trefferzonen |
| Vordergrund | $1{,}00$ bis $1{,}25$ | Schutzscheibe, Kabeltraeger, Transportkorb, Foerdergut und Spänefilter | zeitlich begrenzte, regelbasierte Teilverdeckung |
| HUD | $0$ | Zeit, Punkte, Combo und Modus | nie von Weltobjekten verdeckt |

Der Hintergrund ist eine **Mischform aus prozeduraler Komposition und konkreten Assets**. Wiederholbare Hallenmodule, Bodenraster, Lichtlaeufe und dezente Partikel entstehen aus konfigurierten Kacheln und Farbwerten; sie eignen sich fuer endloses Scrolling und schnelle Layoutvarianten. Werkzeuge, Schutzvorrichtungen, charakteristische Maschinenmerkmale und freigegebene Bildzeichen liegen als PNG-Master und WebP-Auslieferungsasset vor. Laser, Funken, Schleifspuren und kleine Ereignisse verwenden Atlas-/JSON-Animationen mit PNG/WebP-Texturen. Das verhindert sowohl generische prozedurale Werkzeuge als auch eine unflexible, vollstaendig vorgerenderte Panoramawelt.

Verdeckungen sind bewusstes Gameplay, keine zufaellige Renderfolge: Jedes Ziel ist mindestens $650\text{ ms}$ voll sichtbar, maximal $40\,\%$ seiner Trefferflaeche darf fuer hoechstens $450\text{ ms}$ verdeckt sein, und die relevante Schneiden- beziehungsweise Tap-Zone bleibt sichtbar. In den ersten $15\text{ s}$ der Runde gibt es keine Verdeckung; zwischen zwei verdeckten Zielen liegen mindestens $2\text{ s}$. Eine ausbleibende Aktion auf ein verdecktes Ziel kostet weder Punkte noch Combo. Diese Regeln erhalten den humorvollen "gerade noch erwischt"-Moment, ohne die Messeinteraktion unfair zu machen.

Humor entsteht als kurze, optionale Nebeninszenierung der industriellen Welt, nie durch eine Verwechslung mit echten Werkzeugen oder Zielen. Beispiele sind eine uebermotiviert vorbeifahrende Qualitaetsmarkierung, ein zu schnell rollender Schraubenkorb, ein kurz aufblinkendes "scharf genug"-Pruefsiegel oder eine Schutzscheibe, die ein Ziel theatralisch freigibt. Jeder Gag hat ein eigenes sichtbares Ereignis, eine Manifest-ID, ein konfiguriertes Spawnprofil und eine optionale Audioereignisgruppe. Er darf weder Trefferzonen veraendern noch notwendige Gesten verschleiern und ist jederzeit per Konfiguration deaktivierbar.

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

Der neue humoristische Sketch ist die visuelle Referenz fuer Kontrast, Proportionen und die komische Werkzeuginszenierung, nicht die Freigabe fuer dargestellte Wortmarken oder Produktbilder. Die Bedienleiste verwendet keine historischen Produktionsbefehle unveraendert, damit Besucher nicht auf eine echte Maschinensteuerung schliessen. Die blauen Funktionsfelder tragen die fachlichen Bezeichnungen `START`, `LASER`, `SCHEIBE`, `ANLEITUNG`, `HIGHSCORE` und `TON`; die darunterliegenden grauen F-Tasten tragen nur `F1` bis `F6` und sind eine visuelle Retro-Metapher. `F1` ist der einzige Momenttaster und startet nur aus dem Attract- oder Ergebniszustand. `F2` und `F3` sind gegenseitig ausschliessende, rastende Modusschalter: Genau einer ist sichtbar gedrueckt und bestimmt die Runde sowie die zugehoerige Rangliste. `F4`, `F5` und `F6` sind rastende Schalter und behalten ihren sichtbaren Zustand bis zu einer erneuten Betaetigung oder bis zum konfigurierten Ruecksetzen. `F4` aktiviert die kurze, rein visuelle Geste-Demonstration. `F5` oeffnet die getrennte Rangliste des aktiven Modus; waehrend einer laufenden Runde wird sie nicht geoeffnet. `F6` schaltet ausschliesslich lokale Effekte zwischen laut, leise und stumm. Betreiberbefehle bleiben ausserhalb dieser Besucherleiste und folgen weiterhin dem geschuetzten Langdruck mit Slide-to-confirm.

Die Ranglisten `LASER` und `SCHEIBE` sind getrennte, lokal persistent gespeicherte Bestenlisten; ein Eintrag besteht aus Modus, Punktzahl, Name, optionaler E-Mail-Adresse, Zeitstempel und einer technischen Eintrags-ID. Nach jeder regulär beendeten Runde muss vor der Rollenfreigabe mindestens der Name gespeichert werden; Abbruchrunden erzeugen keinen Eintrag. Die optionale E-Mail-Adresse erscheint weder auf Handheld noch Watcher und wird weder zur Lead-Generierung noch fuer automatisierte Kontaktaufnahme verarbeitet. Betreiber koennen beide Ranglisten im geschuetzten Dialog einsehen und lokal loeschen. Die Ausfallwiederherstellung muss die zuletzt bestaetigten Eintraege nach Neustart anzeigen.

Die linke Comicmaschine zeigt als technische Andeutung einen kurzen Portaltraeger mit Schlitten, Kabelschlauch und gemeinsamer Werkzeugaufnahme. Der Laser ist ein schmaler Fokuskopf unter der Aufnahme; die Schleifscheibe haengt seitlich daneben an einem kurzen Schwenkarm mit sichtbarem Schutzring. Im Laser-Modus faehrt der Schlitten entlang einer virtuellen $X$-Achse, richtet sich aus und sendet einen kurzen cyanfarbenen Strahl sichtbar in die gelbe Flaeche. Im Scheiben-Modus schwenkt der Arm vor, die Scheibe dreht an und bleibt mit ihrer Schutzhaube klar als bearbeitendes Werkzeug lesbar. Der Swipe ueber die markierte Werkzeugschneide wird als lineare Zustellung gelesen: Die $X$-Anzeige folgt der Wischrichtung, $Z$ zeigt einen kurzen Zustellimpuls, und ein schmaler Funkenstreifen laeuft entlang der Schneide. Dadurch bleibt die Ninja-Slice-Qualitaet erhalten, ohne dass Besucher eine Scheibe frei zeichnen oder die Maschine selbst steuern muessen.

Die rechten Symboltasten sind als optionale "Werkstatt-Spielereien" ausgestaltet, etwa Messuhr-Zucken, Foerderband-Kurzlauf, Schutzscheiben-Wischer, Pruefsiegel-Stempel oder ein kleiner Werkzeugwechsel. Jede Taste hat ein eindeutiges Piktogramm, einen maximal $2\text{ s}$ langen Effekt, einen konfigurierbaren Cooldown und optionalen Ton. Easter Eggs duerfen erst nach einer konfigurierten Folge harmloser Ereignistasten oder nach einem Highscore erscheinen; sie verbergen weder Ziele noch Navigation, veraendern keine Wertung und sind im Betreiberprofil global deaktivierbar. Die Taste `F6 TON` und die Systemlautstaerke gelten auch fuer alle Spielereien.

Die Referenz ist keine pixelgenaue Reproduktion: Auf Handhelds werden Lesbarkeit und Zielgroessen gegenueber dekorativer Dichte priorisiert. Die Spalten und F-Tasten werden proportional mit festen Mindestgroessen skaliert; bei schmalem Hochformat wird die rechte Ereignisleiste als ausklappbare Symbolschublade angezeigt, waehrend die gelbe Spielflaeche mindestens $70\,%$ der Breite erhaelt. Der Watcher uebernimmt die Arbeitsflaeche und eine grossformatige Maschinenanimation, zeigt jedoch weder F-Tasten noch Ereignistasten als bedienbar an.

# 4. Contracts

## 4.1 tl;dr
- Der Server ist die einzige Autorität für Rolle, Rundentimer, Punktestand und validierte Treffer.
- Clients senden versionierte Befehle, erhalten bestätigte Ereignisse und bei Reconnect vollständige Snapshots.
- Ein Watcher bleibt passiv und kann ohne Einfluss auf die Runde jederzeit wieder beitreten.

| Schnittstelle | Richtung | Datenstruktur / Protokoll |
|-|-|-|
| Sitzungsbeitritt | Client -> Server | `JoinRequest { ClientInstanceId, RequestedRole, ResumeToken? }` über HTTPS/WebSocket; Antwort enthält kurzlebige Berechtigung und `RoleVersion`. |
| Spieleraktion | Aktiver Client -> Server | `ActionRequested { RoundId, CommandSequence, ToolId, ActionKind, InputTimestamp, Gesture }`; `ActionKind` ist `tap` oder `swipe`. |
| Bestätigung | Server -> alle Clients | `ActionResolved { EventId, StateVersion, Outcome, ScoreDelta, ToolState, ServerTimestamp }`; identisch wiederholbar. |
| Zustandssnapshot | Server -> Client | `GameSnapshot { RoundId, StateVersion, Phase, ActiveClientId, Mode, RemainingMs, Score, Combo, Tools }`; bei Join, Reconnect und Versionslücke. |
| Highscore | Aktiver Client -> Server | `HighscoreSubmitted { RoundId, Mode, Name, Email? }`; der Server verlangt einen nichtleeren, validierten Namen, speichert getrennt nach Modus lokal persistent und publiziert nur Name, Punktzahl und Rang. |
| Betrieb | Blazor-Shell -> Server | lokaler Diagnose-/Resetvertrag; der Betreiber-Slide für Freigabe/Abbruch erfordert Betreiberberechtigung und erhöht `RoleVersion`, bei Abbruch zusätzlich `RoundId`. |
| Konfiguration | Host -> Clients | versioniertes Asset-/Gameplay-Manifest, das Kategorie, monotone Asset-ID, Vorlage, Varianten, Ladepfad sowie Bild-/Audioereigniszuordnungen enthält; nur vor einer neuen Runde aktiviert. |

Ein lokaler ASP.NET-Core-Dienst verwendet SignalR/WebSocket für Ereignisse und Snapshots. Transportzustellung ersetzt keine Fachlogik: Der Server dedupliziert `ClientInstanceId + CommandSequence`, prüft `RoundId` und aktive Rolle und beantwortet Wiederholungen idempotent. Ein atomarer Rollenwechsel entzieht zuerst die alte Rolle, erhöht `RoleVersion`, weist dann die neue Rolle zu und sendet einen Snapshot. Die Betreiberaktion `ReleaseRole` ist außerhalb einer Runde zulässig; `AbortRoundAndReleaseRole` beendet die aktive Runde ohne Highscoreeintrag.

# 5. Implementierungsplan

## 5.1 tl;dr
- Die Umsetzung beginnt mit Hardware-/Betriebsentscheidungen und einem vertikalen Prototyp, bevor ein vollständiges Contentpaket entsteht.
- Phaser/TypeScript plus lokaler ASP.NET-Core-Server ist der Favorit, weil Touch-Reaktion und Betriebsverantwortung sauber getrennt bleiben.
- Die Retro-Editor-Huelle ist eine Clientdarstellung mit phasengebundenen Befehlen; sie aendert weder die autoritative Wertung noch die passive Watcherrolle.
- Die nachfolgenden Arbeitspakete sind verbindlich und unverändert in der Fortschrittsseite geführt.

| AP | Ziel / Scope | Betroffene Komponenten | Abhängigkeiten | Validierung | Done-Kriterium |
|-|-|-|-|-|-|
| AP-01 | Messehardware, Netzwerk, Rechte und Spielregeln verbindlich erfassen; Betriebscheckliste erstellen. | Hardwareliste, Netzwerkplan, Asset-/Lizenzregister, Abnahmekriterien | OP-01 bis OP-05 | Review mit Auftraggeber; Vor-Ort-Check mit Originalgeräten | Freigegebene Randbedingungen und testbarer Betriebsplan liegen vor. |
| AP-02 | Reproduzierbares Projektgerüst und lokaler Offline-Host aufsetzen. | .NET-Solution, ASP.NET-Core-Dienst, Blazor-Betriebshülle, TypeScript/Phaser-Client, Build-/Paketierung | AP-01 | sauberer Build auf Zielhost; Start ohne Internet | Ein Befehl startet Server und Watcher; ein Handheld erreicht die lokale Startseite. |
| AP-03 | Autoritativen Runden-, Rollen- und Synchronisationskern implementieren. | C#-Domäne, SignalR-Hub, Snapshots, Betreiberfreigabe/-abbruch, Ereignisse, Konfiguration, MSTest | AP-02 | Unit- und Integrationschecks für Rollen, Betreiberaktionen, Timer, Deduplizierung, Reconnect | Genau ein Client ist aktiv; doppelte oder veraltete Befehle ändern die Wertung nicht. |
| AP-04 | Vertikalen Spielprototyp mit Tap-Laser, Swipe-Schleifen und Retro-Editor-Huelle erstellen. | Phaser-Szenen, gelbe Spielflaeche, linke Maschinen-/Achsanzeige, rechte Ereignisleiste, F-Tastenleiste, 2,5D-Parallaxelandschaft, Touch-Erkennung, lokale Rückmeldung, Watcher-Ansicht | AP-03 | Touch-Test auf Zielgerät; Messung der lokalen Rückmeldung, Zielgroessen, Sichtbarkeitsregeln und phasengebundener Befehle | Beide Gesten funktionieren intuitiv; lokale Rückmeldung liegt im Zielwert, Ziele bleiben vor Verdeckung eindeutig und kein Besucherbefehl beeinflusst Rollen oder Wertung. |
| AP-05 | Inhalte, Attract-/Ergebnisfluss, getrennte persistente Highscores und Asset-/Audiopipeline ausbauen. | Kategorien, Assetvorlagen, Manifeste, Hintergrund-/Vordergrundmodule, Humorereignisse, Tooldefinitionen, HUD, Audio, lokale Ranglisten `LASER`/`SCHEIBE` | AP-04, freigegebene Assets aus AP-01 | Contentwechsel mit neuer ID ohne Codeänderung; Neustart-, Eingabe- und Datenschutzreview | Vollständiger Rundenablauf inklusive Namenspflicht, getrennter stromausfallfester Ranglisten, Rückkehr in Attract, Audio-Fallback, Bild-/Audio-Mapping und validiertem Assetmanifest ist vorhanden. |
| AP-06 | Messehärtung, Fallbacks, Lasttests und Übergabe durchführen. | Kioskmodus, Diagnose, Recovery, Releasepaket, Betriebsdokumentation | AP-05 | Offline-, WLAN-, Reconnect-, Watcher-Ausfall-, Dauerlauf- und Abnahmetest | Abnahmekriterien sind nachweislich erfüllt; Ersatzhost und Wiederanlauf sind geprobt. |

| Option | Vorteile | Nachteile |
|-|-|-|
| A: Blazor Server als vollständiger Spielclient | Durchgängig C#, zentraler Zustand | Touch und Rendering hängen an einer Dauerverbindung; für Partikel/Canvas unnötig fragil. |
| B: Reines Blazor WASM | Offlinefähiges Browser-Frontend, C#-Kenntnisse | Kein Vorteil für den Game-Loop; Mehrgeräte-Autorität bleibt zusätzlich nötig. |
| C: Blazor-Shell + ASP.NET Core + TypeScript/Phaser | Lokaler C#-Betrieb, performanter Canvas-Loop, browserfähige Handhelds, klare Verantwortung | Zwei Frontend-Toolchains benötigen festgeschriebene Builds. |
| D: Reines TypeScript/Phaser | Kleiner Spielstack | Kiosk, Diagnostik und C#-Integration müssen separat gelöst werden. |

**Favorit:** Option C.

**Begründung:** Phaser ist für Canvas, Partikel, Audio und Touch-Gesten der direkte Laufzeitfit; ASP.NET Core und Blazor geben dem überwiegend C#/C++-erfahrenen Team eine robuste Heimat für Regeln, Diagnose und lokalen Messebetrieb. Gegenüber A bleibt die unmittelbare Touch-Reaktion vom Netz entkoppelt; gegenüber D erhält der Betrieb einen klaren, wartbaren Host.

Empfohlene Werkzeuge: Phaser als 2D-Laufzeit-Engine, TypeScript für Clientcode, .NET/ASP.NET Core für Server und Shell, optional Blender für 3D-Quellmodelle, Figma oder vergleichbares freigegebenes Tool für UI, und eine DAW für selbst produzierte Audioassets. KI-Werkzeuge können Moodboards oder Entwürfe unterstützen, dürfen aber nur nach Rechte- und Markenprüfung in Assets überführt werden. C++ bleibt für einen später nachgewiesenen nativen Algorithmus reserviert; keine Browser-DLL-Annahme.

# 6. Tests

## 6.1 tl;dr
- Der Schwerpunkt liegt auf messbaren Touch-, Synchronisations-, Performance- und Messebetriebsprüfungen auf echter Zielhardware.
- C#-Regeln werden mit MSTest abgedeckt; Client- und Browserflüsse erhalten automatisierte E2E-Checks sowie manuelle Hardwareabnahme.
- Kein Akzeptanzwert gilt als erfüllt, bevor er in einer reproduzierbaren Konfiguration gemessen wurde.

| Bereich | Prüffall / Akzeptanzkriterium |
|-|-|
| Rundenlänge | Standardrunde endet serverautoritativ nach $60\text{ s}$; konfigurierbarer Bereich lässt nur $30$ bis $90\text{ s}$ zu. |
| Touch | Tap und Swipe erzeugen lokal sichtbares Feedback in $\leq 50\text{ ms}$ p95 auf dem Zielhandheld. |
| Framerate | Aktiver Client erreicht $\geq 55\text{ FPS}$ p95, Watcher $\geq 50\text{ FPS}$ p95 unter dem freigegebenen Maximalspawnprofil. |
| Hintergrund und Verdeckung | Jedes Ziel ist mindestens $650\text{ ms}$ vor der ersten Verdeckung vollständig sichtbar; keine Verdeckung überschreitet $40\,\%$ Trefferfläche oder $450\text{ ms}$. |
| Retro-Editor-Huelle | Die gelbe Arbeitsflaeche ist der einzige Ziel- und Gestenbereich; linke Achsen reagieren sichtbar auf Laser und Schleifswipe, ohne selbst Eingabe zu werden. |
| Funktions- und F-Tastenleiste | `F1` ist ein nicht rastender Starttaster. `F2` bis `F6` sind ausreichend grosse, visuell rastende Schalter; `F2 LASER` und `F3 SCHEIBE` bleiben stets gegenseitig ausschliessend, genau einer ist aktiv. Die grauen Tasten enthalten ausser `F1` bis `F6` keinen weiteren Text. Alle rechten Ereignistasten sind wertungsneutral und respektieren `TON`. |
| Laser- und Schleifscheiben-Inszenierung | Die linke Maschine zeigt Portaltraeger, Schlitten, Kabelschlauch, Fokuskopf sowie eine seitliche Scheibe mit Schutzring. Ein Swipe entlang der sichtbaren Schneide zeigt gerichtete Achsbewegung, Scheibenanlauf und Funkenstreifen, ohne eine freie Maschinensteuerung oder unklare Trefferzone zu erzeugen. |
| Kleine Viewports | Im schmalen Hochformat bleibt die gelbe Spielflaeche mindestens $70\,%$ breit; die Ereignisleiste wechselt ohne Ueberlappung in eine Symbolschublade. |
| Highscore und Neustart | Nach einer regulären Runde verhindert ein leerer Name die Rollenfreigabe. `LASER` und `SCHEIBE` speichern und zeigen getrennte Ranglisten; alle bestaetigten Eintraege bestehen einen kontrollierten Hostneustart. Optionale E-Mail-Adressen bleiben aus jeder oeffentlichen Anzeige ausgeschlossen. |
| Synchronisierung | Befehl bis Server p95 $\leq 75\text{ ms}$, bestätigter Clientzustand p95 $\leq 150\text{ ms}$, Watcher p95 $\leq 250\text{ ms}$ im Messe-WLAN. |
| Rollen | Zwei parallele Beitrittsversuche ergeben stets genau eine aktive Rolle; nur der Betreiber-Slide kann freigeben oder abbrechen, und jeder Wechsel ist atomar und versioniert. |
| Paketverlust | Wiederholte Nachricht und Versionslücke ändern Punktestand/Ereignis nie doppelt; Snapshot stellt Konsistenz wieder her. |
| Offline | Start, Runde, Watcher und Assetladen funktionieren ohne WAN-Verbindung. |
| Asset-Austausch | Ein validiertes Manifest nimmt ein neues Asset mit nächster freier Kategorie-ID und passender Vorlage vor einer Runde ohne Kernlogikänderung auf; doppelte, fehlende oder vorlagenwidrige IDs blockieren die Aktivierung. |
| Bild-/Audio-Mapping | Ein neues Hintergrund-, Humor- oder Ereignisasset referenziert seine optionale Audioereignisgruppe allein im Manifest; bei fehlendem Audio bleibt das Bildereignis ohne Unterbrechung sichtbar. |
| Fallback | Watcher-Neustart beeinflusst die Runde nicht; Handheld-Reconnect folgt der definierten Frist; Serverausfall führt zu kontrolliertem Neustart oder getrenntem Demo-Modus. |

Unit-Tests sichern Punkte-, Combo-, Zeit-, Rollen- und Deduplizierungsregeln ab. Integrationstests prüfen Hub-/Snapshotverträge, Konkurrenz und Reconnect. E2E-Tests decken Start, Gesten, Runde, Ergebnis und Watcherbeitritt ab. Zusätzlich sind ein Dauerlauf über mindestens vier Stunden, ein Paketverlusttest, ein Kaltstarttest und eine Vor-Ort-Abnahme mit finalen Geräten verpflichtend.

# 7. Risiken

| Risiko | Kurzbeschreibung | Risikohöhe |
|-|-|-|
| R-01 | Ungeeignetes WLAN oder Touch-Hardware überschreitet das Latenzbudget. | hoch |
| R-02 | Nicht freigegebene Marken-, Produkt-, Bild- oder Audioassets verhindern die Auslieferung. | hoch |
| R-03 | Hohe Partikeldichte oder ungetestete Browserkonfiguration senkt die Bildrate. | mittel |
| R-04 | Serverausfall unterbricht die gemeinsame Wertungsrunde. | mittel |

## 7.1 R-01: Messehardware und Netzreaktion
### tl;dr
- **Favorit:** Originalhardware und dedizierten Access Point früh messen und vor Ort erneut abnehmen.

### Detail
Besucher-WLAN, Browser-Energiesparen und unbekannte Touchpanels sind für wahrgenommene Latenz entscheidender als eine theoretische Frameworkwahl.

### Empfehlung
- **Favorit:** Ein isoliertes Messe-WLAN mit eigenem Access Point, fester Hostadresse und getesteten Zielgeräten.
- **Begründung:** Damit werden Störquellen und Aufbauvarianten gegenüber fremdem Messe-LAN deutlich reduziert.

## 7.2 R-02: Asset- und Lizenzfreigaben
### tl;dr
- **Favorit:** Assetregister und schriftliche Freigabe vor Contentproduktion abschließen.

### Detail
Das Repository enthält eine MIT-Lizenz mit abweichender Namensnennung; Corporate Assets und Medien liegen nicht vor.

### Empfehlung
- **Favorit:** Nur Vollmer-freigegebene, selbst produzierte oder eindeutig kommerziell lizenzierte Assets ins Release aufnehmen.
- **Begründung:** Eine dokumentierte Rechtekette verhindert späte Austauschpflichten und rechtliche Risiken.

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

# 8. Offene Punkte

| Offener Punkt | Kurzbeschreibung | Status | Favorit / Nächste Aktion | Begründung |
|-|-|-|-|-|
| OP-01 | Zielhardware und Aufstellungsdetails | ❌ zu klären | Originalgeräte, Anschlüsse und Kioskmodus verbindlich erfassen. | Hardware bestimmt Browser, Auflösung, Audio und Performancebudget. |
| OP-02 | Messe-Netzwerk und Verantwortlichkeit | ❌ zu klären | Eigenen Access Point mit isolierter SSID einsetzen und vor Ort testen. | Lokale Kontrolle senkt Latenz- und Zugriffsrisiken. |
| OP-03 | Corporate Assets, Lizenz und Rechte | ❌ zu klären | Freigabeprozess und Assetregister durch Vollmer benennen. | Der Bestand enthält keine verwertbaren Medien und uneinheitliche Rechtehinweise. |
| OP-04 | Highscore, Pflichtname und optionale E-Mail-Adresse | ❌ zu klären | Aufbewahrungsfrist, Hinweistext, Betreiberzugriff und Loeschprozess verbindlich freigeben. | Namen und optionale E-Mail-Adressen werden lokal persistiert und muessen datenschutzkonform behandelbar sein. |
| OP-05 | Fachliche Regeln und Erfolgskriterien | ❌ zu klären | Workshop mit Produkt-/Messeverantwortlichen, danach prototypische Abnahme. | Werkzeugzuordnung, Tonalität und Schwierigkeitskurve brauchen Fachfreigabe. |
| OP-06 | Technologieempfehlung | ✅ geklärt | Option C als Ausgangsarchitektur planen. | Sie erfüllt Touch-, Offline- und Betriebsanforderungen am ausgewogensten. |

## 8.1 ❌ OP-01: Zielhardware und Aufstellungsdetails
### tl;dr
- Fehlende Information: Handheldmodell, OS/Browser, Watcherauflösung, PC-Spezifikation, Audio und Stromversorgung.

### Detail
Ohne diese Daten sind FPS-, Auflösungs- und Kioskannahmen nicht prüfbar.

### Empfehlung
- **Favorit:** Eine verbindliche Hardwareliste inklusive Ersatzgerät und Anschlusstest erstellen.
- **Begründung:** Sie ist Voraussetzung für belastbare Performance- und Fallbackabnahme.
- **Nächste Aktion:** AP-01 führt einen Kaltstart- und Anschlusscheck mit Originalgeräten durch.

## 8.2 ❌ OP-02: Messe-Netzwerk und Verantwortlichkeit
### tl;dr
- Fehlende Information: Nutzung und Freigabe eines eigenen WLANs am Messestand.

### Detail
Die Mehrgerätearchitektur benötigt lokale Erreichbarkeit, aber kein Internet.

### Empfehlung
- **Favorit:** Eigenen Access Point betreiben, Besucherzugang davon trennen und den Kanal vor Ort prüfen.
- **Begründung:** Das schafft die kontrollierbarste Latenz und Ausfallsituation.
- **Nächste Aktion:** Netzwerkverantwortliche Person und zulässige Funkkonfiguration festlegen.

## 8.3 ❌ OP-03: Corporate Assets, Lizenz und Rechte
### tl;dr
- Fehlende Information: Freigegebene Vollmer-Assets sowie Rechte an Medien und Auslieferung.

### Detail
Es sind keinerlei Produktbilder, Logos, Schriften oder Sounds im Repository vorhanden; die bestehende Lizenzzuordnung bedarf Prüfung.

### Empfehlung
- **Favorit:** Ein versionsgeführtes Assetregister mit Quelle, Lizenz, Freigabe, Verwendungszweck und Ablaufdatum führen.
- **Begründung:** Inhalte bleiben kurzfristig austauschbar und rechtlich nachvollziehbar.
- **Nächste Aktion:** Vollmer benennt Marke-/Rechteverantwortliche und liefert Freigabepaket.

## 8.4 ❌ OP-04: Highscore und Personendaten
### tl;dr
- Festgelegt: Ein Name ist fuer jeden Highscoreeintrag verpflichtend; die Ranglisten `LASER` und `SCHEIBE` bleiben lokal ueber einen Neustart hinaus erhalten.
- Fehlende Information: Aufbewahrungsfrist, Datenschutzhinweis, Zugriffskreis und Loeschprozess fuer Namen und optional eingegebene E-Mail-Adressen.

### Detail
Der Name wird oeffentlich zusammen mit Punktzahl und Rang angezeigt. Eine E-Mail-Adresse ist freiwillig, wird nicht angezeigt, nicht fuer automatisierte Kontaktaufnahme verwendet und erzeugt keinen Leadprozess; Messeleads entstehen ausschliesslich im persoenlichen Gespraech. Beide Datenarten werden lokal persistent gespeichert und koennen daher nicht mehr als rein anonyme Tagesliste behandelt werden.

### Empfehlung
- **Favorit:** Eingabe auf einen validierten Anzeigenamen begrenzen, E-Mail nur als klar optionales Feld erfassen, beide lokalen Ranglisten ueber den Betreiber loeschbar halten und vor Eingabe einen freigegebenen Datenschutzhinweis mit Aufbewahrungsfrist anzeigen.
- **Begründung:** Das erfuellt die gewuenschte langlebige Highscorefunktion, minimiert die erhobenen Daten und trennt das Spiel eindeutig von der persoenlichen Leadgenerierung.
- **Nächste Aktion:** Datenschutz- und Messeverantwortliche geben Zeichenregeln, Aufbewahrungsfrist, Hinweistext, Betreiberzugriff und sicheren lokalen Loeschprozess frei.

## 8.5 ❌ OP-05: Fachliche Regeln und Erfolgskriterien
### tl;dr
- Fehlende Information: Freigegebene Werkzeugtypen, Markenansprache, Punktebalance und Messeziel.

### Detail
Die Laser-/Schleifzuordnung ist im Konzept bewusst plausibel, aber nicht als fachlich bestätigte Aussage formuliert.

### Empfehlung
- **Favorit:** Einen kurzen Entscheidungsworkshop mit Produktmarketing, Anwendungstechnik und Messebetrieb durchführen und danach den vertikalen Prototyp abnehmen.
- **Begründung:** So wird eine verständliche Arcade-Mechanik mit einer fachlich glaubwürdigen Darstellung verbunden.
- **Nächste Aktion:** Workshoptermin und Abnahmeszenario vor AP-02 festlegen.

## 8.6 ✅ OP-06: Technologieempfehlung
### tl;dr
- Die Entscheidung ist als Konzeptempfehlung getroffen, ihre Realisierungsdetails werden in AP-02 validiert.

### Detail
Blazor Server ist kein geeigneter primärer Echtzeit-Client; reines Phaser löst den lokalen Messebetrieb weniger vollständig. Option C trennt diese Verantwortungen.

### Empfehlung
- **Favorit:** Lokaler ASP.NET-Core-Server, Phaser/TypeScript-Clients, Blazor-Shell für Betrieb und Watcher-Start.
- **Begründung:** Sie kombiniert direkte Touch-Interaktion, lokale Autorität und anschlussfähige C#-Wartung.
- **Nächste Aktion:** AP-02 validiert Start, Paketierung und Kioskmodus auf Zielhardware.
