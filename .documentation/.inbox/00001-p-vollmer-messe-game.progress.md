# Fortschritt: Vollmer Messe-Game
| | |
|-|-|
| **Konzept:** | [Vollmer Messe-Game](00001-c-vollmer-messe-game.concept.md) |
| **Letzte Änderung:** | 2026-09-09 |

Arbeitspakete aus Kap. 5 des Konzepts. Diese Tabelle ist die verbindliche Arbeitsliste für die Umsetzung.
Status: `⬜ offen` / `🔄 in Arbeit` / `✅ erledigt` / `⛔ blockiert`

| AP | Beschreibung | Status | Erledigt am | Nachweis / Nächster Schritt |
|-|-|-|-|-|
| AP-01 | Messehardware, Netzwerk, Rechte und Spielregeln verbindlich erfassen; Betriebscheckliste erstellen. | ⬜ offen | | Freigegebene Randbedingungen und testbarer Betriebsplan liegen vor. |
| AP-02 | Reproduzierbares Projektgerüst und lokaler Offline-Host aufsetzen. | ⬜ offen | | Ein Befehl startet Server und Watcher; ein Handheld erreicht die lokale Startseite. |
| AP-03 | Autoritativen Runden-, Rollen- und Synchronisationskern implementieren. | ⬜ offen | | Genau ein Client ist aktiv; doppelte oder veraltete Befehle ändern die Wertung nicht. |
| AP-04 | Vertikalen Spielprototyp mit Tap-Laser und Swipe-Schleifen erstellen. | ⬜ offen | | Beide Gesten funktionieren intuitiv; lokale Rückmeldung liegt im Zielwert und Ziele bleiben vor Verdeckung eindeutig. |
| AP-05 | Inhalte, Attract-/Ergebnisfluss, Highscore und Asset-/Audiopipeline ausbauen. | ⬜ offen | | Vollständiger Rundenablauf inklusive Rückkehr in Attract, Audio-Fallback, Bild-/Audio-Mapping und validiertem Assetmanifest ist vorhanden. |
| AP-06 | Messehärtung, Fallbacks, Lasttests und Übergabe durchführen. | ⬜ offen | | Abnahmekriterien sind nachweislich erfüllt; Ersatzhost und Wiederanlauf sind geprobt. |

Regeln:
- Vor der Bearbeitung das aktuelle Arbeitspaket auf `🔄 in Arbeit` setzen; gleichzeitig darf höchstens ein Arbeitspaket in Arbeit sein.
- `✅ erledigt` erst nach umgesetztem Code, erforderlicher Validierung und betroffenen Dokumentationsänderungen setzen. Den Test-/Build-Nachweis in der letzten Spalte festhalten.
- Bei `⛔ blockiert` Ursache, benötigte Entscheidung oder fehlende Voraussetzung sowie den exakten Wiedereinstieg in der letzten Spalte festhalten.
- Bei Unterbrechung bleibt das aktuelle Arbeitspaket `🔄 in Arbeit`; die letzte Spalte beschreibt den konkreten nächsten Schritt.
