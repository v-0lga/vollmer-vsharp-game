# [Kurzer deutscher Titel]
|||
|-|-|
| **Erstelldatum:** | [heutiges Datum] |
| **Letzte Änderung:** | [heutiges Datum] |
| **Issue:** | [Concept-Counter, z. B. 00025] |
| **Task:** | XXXXX (Link) |
| **PBI:** | XXXXX (Link) |
| **Ausarbeitung:** | Titel (Link) *(optional)* |

## Commit-Vorschlag für den finalen Gesamt-Commit

> **Status:** `vorläufig` — Der Vorschlag wird nach Umsetzung, Einarbeitung aller Code-Review-Findings und erfolgreicher finaler Review anhand der tatsächlich enthaltenen Änderungen aktualisiert. Es wird kein separater Konzept-Commit erstellt.

```text
[00025] Logik zentralisieren

- Geplante Umsetzung: [kurze Scope-Zusammenfassung]
- Geplante Entscheidung: [wichtigste Architektur-/Umsetzungsentscheidung]
- Geplante Absicherung: [wichtigste Tests oder Validierung]
- Offene Punkte: [wichtigster offener Punkt oder "keine blockierenden offenen Punkte"]
```

Regeln zur Kopf-Tabelle:
- `Issue` wird standardmäßig mit dem Concept-Counter befüllt (z. B. `00025`), wenn keine explizite Issue-Referenz angegeben wurde
- `Task`, `PBI` nur aufnehmen, wenn vom User explizit angegeben
- Leere Fallback-Zeile `| **Issue:** | |` nur verwenden, wenn der Counter ausnahmsweise nicht ermittelbar ist
- `Ausarbeitung` nur aufnehmen, wenn vom User explizit angegeben
- Der `Commit-Vorschlag` steht ausschließlich im Abschnitt `Commit-Vorschlag für den finalen Gesamt-Commit`; die Kopf-Tabelle enthält keine doppelte Commit-Information
- Während der Konzeptphase bleibt der Status `vorläufig`; die Stichpunkte beschreiben nur geplante Änderungen
- Nach der finalen Review wird der Status auf `final` gesetzt und der Text auf die tatsächlich enthaltenen Implementierungs-, Test-, Dokumentations- und Review-Änderungen aktualisiert
- Der Betreff beginnt mit dem Concept-Counter; weitere Formatkonventionen richten sich nach der beim finalen Commit tatsächlich betroffenen Änderung

# 0. Entscheidungsvorlage
> Maximal 5 Zeilen. Kein Fließtext. Kein Detail. Wer nur Kap. 0 liest, weiß ob er zustimmen oder nachfragen muss.

| | |
|-|-|
| **Problem** | Ein Satz. |
| **Lösung** | Ein Satz. |
| **Entscheidungsbedarf** | Was muss freigegeben oder entschieden werden? |
| **Top-Risiko** | Höchstes Risiko aus Kap. 7 in einem Satz (`— entfällt —` wenn kein Risiko). |
| **Blockierende offene Punkte** | ❌-Punkte aus Kap. 8, die vor Umsetzung zu klären sind (`— entfällt —` wenn keine). |

# 1. Kontext, Zielsetzung & Use Case
## 1.1 tl;dr
- ...

## 1.2 Ziele und Nicht-Ziele
| Ziel | Beschreibung |
|-|-|
| Z-01 | ... |

| Nicht-Ziel (Scope) | Begründung |
|-|-|
| NZ-01 | Warum wird dieses Problem bewusst nicht gelöst? |

- Hintergrund und Auslöser (Bug, Feature, Anforderung)
- Relevante Use Cases
- Begriffsdefinitionen *(nur wenn notwendig)*

# 2. Analyse / Ist-Zustand
## 2.1 tl;dr
- ...

- Aktueller Zustand in Code oder Architektur
- Betroffene Komponenten, Klassen, Schnittstellen
- Kernproblem bzw. identifizierte Lücke
- Code-Snippets oder Mermaid-Diagramme *(nur wenn zum Verständnis nötig)*

# 3. Lösungskonzept / Soll-Zustand
## 3.1 tl;dr
- ...

## 3.2 Nicht-Ziele
| Nicht-Ziel (Implementierung) | Begründung |
|-|-|
| NZ-01 | Was wird in dieser Lösung bewusst nicht umgesetzt? |

- Angestrebter Zustand nach der Umsetzung
- Wesentliche Designentscheidungen

# 4. Contracts
## 4.1 tl;dr
- ...

- Relevante Kommunikations-Verträge (REST-APIs, Events, Datenformate, Protokolle oder Broker oder Messaging-Systeme wie z.B. Redis)
- Pro Schnittstelle: Richtung, Datenstruktur, Protokoll

# 5. Implementierungsplan
## 5.1 tl;dr
- ...

- Arbeitspakete in festgelegter Reihenfolge. Jedes Arbeitspaket ist ein einzeln wiederaufnehmbarer Umsetzungsschritt und wird unverändert in die zugehörige `.progress.md` übernommen.

| AP | Ziel / Scope | Betroffene Komponenten | Abhängigkeiten | Validierung | Done-Kriterium |
|-|-|-|-|-|-|
| AP-01 | ... | ... | keine / AP-## | Build/Test/Prüfung | Konkreter prüfbarer Endzustand |

- Ein Arbeitspaket erst abschließen, wenn Code, erforderliche Tests und betroffene Dokumentation umgesetzt sind.
- Mermaid-Diagramme *(falls dem Verständnis zuträglich)*
- Falls mehrere Ansätze existieren: kurzer Vergleich + klar markierter Favorit mit Begründung anhand mindestens zweier relevanter Kriterien

| Option | Vorteile | Nachteile |
|-|-|-|
| A | ... | ... |
| B | ... | ... |

**Favorit:** Option A — konkrete Entscheidung oder Maßnahme.

**Begründung:** Warum Option A gegenüber den Alternativen vorzuziehen ist.

# 6. Tests
## 6.1 tl;dr
- ...

- Geplante Teststufen: Unit / Integration / E2E
- Testfälle und Akzeptanzkriterien
- Relevante Randbedingungen oder Testdaten

# 7. Risiken
Nomenklatur: `R-##`

Regeln:
- Dieses Kapitel enthält nur Risiken, für die eine konkrete Empfehlung möglich ist
- Jede Risikoempfehlung muss einen klaren Favoriten und dessen Begründung benennen; `Nichts tun` ist zulässig, wenn es bewusst begründet wird
- Unklare Sachverhalte, fehlende Entscheidungen oder offene Klärungen gehören in Kapitel 8

| Risiko | Kurzbeschreibung | Risikohöhe |
|-|-|-|
| R-01 | ... | kritisch / hoch / mittel / kosmetisch |

## 7.1 R-01: ...
### tl;dr
- Kurze Zusammenfassung

### Detail
- Detaillierte Ausführung mit Hintergrund
- Klickbare Links auf Quellcode, externe Dokumentation und Abschnitte dieses Dokuments sind ausdrücklich erwünscht, sofern zweckdienlich

### Empfehlung
- **Favorit:** Konkrete Maßnahme.
- **Begründung:** Warum diese Maßnahme gegenüber Alternativen vorzuziehen ist.

## 7.2 R-02: ...
etc.

# 8. Offene Punkte
Nomenklatur: `OP-##`

Regeln:
- Dieses Kapitel enthält alle ungeklärten Sachverhalte, fehlenden Entscheidungen und offenen Klärungen
- Jede Überschrift in Kapitel 8 kennzeichnet nach der Abschnittsnummer mit `✅` oder `❌`, ob der Punkt geklärt oder noch offen ist
- Jeder offene Punkt enthält einen konkreten Favoriten und dessen Begründung. Nur wenn eine entscheidungserhebliche Information fehlt, bleibt die Entscheidung offen; dann fehlende Information, Beschaffungsweg und vorläufige Handlung mit Begründung angeben.

| Offener Punkt | Kurzbeschreibung | Status | Favorit / Nächste Aktion | Begründung |
|-|-|-|-|-|
| OP-01 | ... | ❌ zu klären | ... | ... |
| OP-02 | ... | ✅ geklärt | ... | ... |

## 8.1 ❌ OP-01: ...
### tl;dr
- Kurze Zusammenfassung

### Detail
- Detaillierte Ausführung mit Hintergrund
- Klickbare Links auf Quellcode, externe Dokumentation und Abschnitte dieses Dokuments sind ausdrücklich erwünscht, sofern zweckdienlich

### Empfehlung
- **Favorit:** Konkrete Entscheidung oder Handlung.
- **Begründung:** Warum dieser Weg gegenüber Alternativen vorzuziehen ist.
- **Nächste Aktion:** Konkreter Beschluss, Prüfschritt oder Beschaffungsweg für fehlende entscheidungserhebliche Information.

## 8.2 ✅ OP-02: ...
etc.