# Demomaterial fuer Assettypen

Diese Dateien sind ein visuelles Konzept- und Abstimmungspaket. Sie enthalten echte lokal erzeugte PNG- und WAV-Platzhalter, sind aber keine Runtime-Assets und ersetzen weder die im Konzept geforderten Freigaben noch die spaetere Asset-Validierung. Ihre technische und gestalterische Qualität ist ausdrücklich kein Maßstab für die Produktionsqualität des Games: Sie demonstrieren allein Struktur, Kategorien, IDs, Ladepfade und Manifestreferenzen.

## Anzeige

[asset-gallery.html](asset-gallery.html) direkt im Browser oeffnen. Die Datei funktioniert lokal ohne Server und laedt fuer jede definierte Assetkategorie ein Beispiel aus `Assets/`. Die vier WAV-Dateien sind bewusst lokal synthetisierte Platzhalter, damit keine nicht freigegebenen Musik- oder Sounddateien in das Repository gelangen.

## Enthaltene Kategorien

| Kategorie | Beispiel-ID | Produktionsformat laut Konzept | Darstellung in der Galerie |
|-|-|-|-|
| Werkzeugbild | `tool_0001` | PNG oder WebP | PNG-Bohrer mit `dull`-/`sharp`-Zustand |
| Lasereffekt | `fx_0001` | Phaser-Partikelatlas mit JSON plus PNG-Textur | PNG-Laserimpuls |
| Schleifeffekt | `fx_0001` | Phaser-Partikelatlas mit JSON plus PNG-Textur | PNG-Schleifkontakt mit Funken |
| Watcher-Attract | `attract_0001` | WebM oder PNG-Standbild | PNG-Konzeptbild fuer Attract |
| Laser-SFX | `sfx_0001` | OGG oder WAV | WAV mit kurzem hohem Sinusimpuls |
| Schleif-SFX | `sfx_0001` | OGG oder WAV | WAV mit kurzem Schleifimpuls |
| Musik-Loop | `music_0001` | OGG oder WAV | WAV mit dezentem Dreiklang |
| UI-SFX | `sfx_0001` | OGG oder WAV | WAV mit zweistufigem Rundenende-Signal |

## Namens- und Manifestregel

Der Dateiname besteht ausschließlich aus Kategoriepraefix und vierstelliger, monotoner ID. Die Bedeutung steht im [asset-manifest.demo.json](asset-manifest.demo.json), etwa Interaktion, Zustand, Varianten, Rechte und Ladepfad. IDs werden pro Kategorie nie wiederverwendet.

Die Datei [asset-manifest.demo.json](asset-manifest.demo.json) ist nur ein lesbares Beispielschema. In der Umsetzung wird daraus ein versioniertes, durch den Manifest-Validator geprueftes Produktionsmanifest.
