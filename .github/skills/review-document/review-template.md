# [Kurzer deutscher Titel]

|||
|-|-|
| **Erstelldatum:** | [heutiges Datum] |
| **Letzte Änderung:** | [heutiges Datum] |
| **Issue:** | [Concept-Counter des referenzierten Konzepts, z. B. 00025] |
| **Task:** | XXXXX (Link) |
| **PBI:** | XXXXX (Link) |

Regeln zur Kopf-Tabelle:
- `Issue` wird standardmäßig mit dem Counter des referenzierten Konzepts befüllt, wenn keine explizite Issue-Referenz angegeben wurde
- `Task`, `PBI` nur bei expliziter User-Angabe
- Leere Fallback-Zeile `| **Issue:** | |` nur verwenden, wenn der Counter ausnahmsweise nicht ermittelbar ist

# 1. Scope & Kontext
- Review-Umfang (PR, Dateien, Modul)
- Ziel und Randbedingungen

# 2. Zusammenfassung
- Gesamtbewertung: Approve / Request Changes / Needs Discussion
- Anzahl Findings je Severity

# 3. Findings (Critical)
- ID, Ort, Problem, Risiko, Empfehlung

# 4. Findings (Major)
- ID, Ort, Problem, Risiko, Empfehlung

# 5. Findings (Minor/Suggestion)
- ID, Ort, Problem, Empfehlung

# 6. Test- & Regressionssicht
- Abdeckungslücken, notwendige Tests, Regression-Risiken

# 7. Risiken
> Nur falls erforderlich, Reviews enthalten planmäßig nur harte Empfehlungen, keine offenen Fragen.

Nomenklatur: `R-##`

Regeln:
- Dieses Kapitel enthält nur Risiken, für die eine konkrete Empfehlung möglich ist
- Jede Risikoempfehlung muss klar benennen, was getan werden soll; `Nichts tun` ist zulässig, wenn es bewusst begründet wird
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
> Nur falls erforderlich, Reviews enthalten planmäßig nur harte Empfehlungen, keine offenen Fragen.

Nomenklatur: `OP-##`

Regeln:
- Dieses Kapitel enthält alle ungeklärten Sachverhalte, fehlenden Entscheidungen und offenen Klärungen
- Jede Überschrift in Kapitel 8 kennzeichnet nach der Abschnittsnummer mit `✅` oder `❌`, ob der Punkt geklärt oder noch offen ist
- Jeder offene Punkt enthält einen konkreten Favoriten und dessen Begründung. Nur wenn eine entscheidungserhebliche Information fehlt, bleiben Favorit und Entscheidung offen; dann fehlende Information, Beschaffungsweg und vorläufige Handlung mit Begründung angeben.

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