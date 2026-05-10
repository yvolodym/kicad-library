# Electrical KiCad Library

KiCad-Bibliothek mit Symbolen und Footprints für Bauteile, die in den
Standard-Bibliotheken fehlen oder bei denen mir die vorhandenen Varianten nicht
gepasst haben. Die Bibliothek wird als KiCad-Package-Manager-(PCM-)Addon
ausgeliefert.

- **KiCad-Version:** 9.0.0+
- **Lizenz:** [CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## Inhalt

| Symbol                            | Footprint                                                  | Bemerkung |
| --------------------------------- | ---------------------------------------------------------- | --------- |
| `8205A`                           | _stock_ `Package_TO_SOT_SMD:SOT-23-6`                      | Dual-N-Kanal-MOSFET, [Datenblatt](https://www.umw-ic.com/static/pdf/17bbbd598f6f936a3c6d46378372fe52.pdf) |
| `CH340B`                          | _stock_ `Package_SO:JEITA_SOIC-16_3.9x9.9mm_P1.27mm`       | USB-zu-Seriell-Wandler |
| `ESP32-WROVER-IB`                 | `PCM_yvolodym:ESP32-WROVER-B`                              | Espressif WiFi/BT-Modul |
| `Joystick_Analog`                 | `PCM_yvolodym:Joystick_Analog`                             | Generischer analoger Joystick |
| `Joystick_PS5`                    | `PCM_yvolodym:Joystick_PS5`                                | Alps [RKJXV122400R](https://tech.alpsalpine.com/e/products/detail/RKJXV122400R/) (PS5-Style) |
| `MODULE-SEEEDUINO-XIAO-ESP32C3`   | `PCM_yvolodym:MODULE-SEEEDUINO-XIAO-ESP32C3`               | Seeed Studio XIAO ESP32-C3 Board |
| `NCR18650B`                       | `PCM_yvolodym:NCR18650B`                                   | Panasonic 18650-Zellenhalter-Layout |
| `Switch small`                    | `PCM_yvolodym:Switch`                                      | Kleiner Taster |

## Installation

### Variante 1: Über den KiCad Package Manager (empfohlen)

1. KiCad öffnen → **Plugin and Content Manager** starten.
2. Tab **Plugins** → Bibliothek `Electrical Library` suchen → **Install** klicken.
3. KiCad neu starten. Symbol- und Footprint-Bibliothek werden automatisch unter
   dem Nicknamen `PCM_yvolodym` registriert.

### Variante 2: Manuelle Installation

1. Das aktuelle `yvolodym-kicad-addon.zip` aus dem
   [neuesten GitHub-Release](https://github.com/yvolodym/kicad-library/releases/latest)
   herunterladen.
2. In KiCad **Preferences → Manage Symbol Libraries** und
   **Manage Footprint Libraries** öffnen, jeweils einen Eintrag mit Nickname
   `PCM_yvolodym` hinzufügen, der auf den ausgepackten Pfad zeigt
   (`symbols/yvolodym.kicad_sym` bzw. `footprints/yvolodym.pretty/`).

> **Achtung:** Wenn die Bibliothek manuell unter einem anderen Nicknamen
> eingebunden wird, finden die Symbole ihre Footprints nicht — die
> `Footprint`-Properties in der `.kicad_sym`-Datei sind hart auf
> `PCM_yvolodym:` verdrahtet.

## Release-Prozess

Releases werden über den GitHub-Actions-Workflow **Release Addon** gebaut.
Eingabe der gewünschten Versionsnummer (z. B. `1.0.4`) genügt — der Workflow
baut die Zip-Datei, taggt das Release und committet `metadata.json` zurück. Der
manuelle Schritt für die KiCad-PCM-Metadaten auf GitLab ist in
[`release-kicad-addons.md`](release-kicad-addons.md) beschrieben.

## Bibliotheksaufbau

```
symbols/yvolodym.kicad_sym                 ← alle Symbole in einer Datei
footprints/yvolodym.pretty/*.kicad_mod     ← alle Footprints
3dmodels/yvolodym.3dshapes/                ← (aktuell leer)
resources/icon.png                         ← PCM-Icon
```

Symbol-, Footprint- und 3D-Modell-Namen sollen für jedes Bauteil identisch
sein (Triplet-Konvention).
