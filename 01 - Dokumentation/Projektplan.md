# 📅 Projektplan – WordPress CMS Migration LB2

## Übersicht

Hier ist mein Plan für dieses Projekt über die neun Wochen. Dieser zeigt alle Phasen, inklusive Snapshots und Meilensteinen.

## 📊 Zeitplan

# 📅 Projektplan – WordPress CMS Migration LB2 (9 Wochen)

## Gantt-Diagramm

```mermaid
gantt
    title WordPress CMS Migration – Projektplan Yenul LB2
    dateFormat  DD.MM.YYYY
    axisFormat  %d.%m.

    section Phase 1: Planung
    Projektstart           :start, 15.05.2025, 1d
    Backup holen           :backup, 22.05.2025, 1d
    Plan fertig            :milestone, plan_done, 23.05.2025, 0d

    section Phase 2: Umgebung
    VM erstellen           :vm, 29.05.2025, 1d
    Snapshot erstellen 1   :snap1, 29.05.2025, 1d
    LAMP Setup             :lamp, 05.06.2025, 1d
    DNS einrichten         :dns, 06.06.2025, 1d
    Snapshot erstellen 2   :snap2, 06.06.2025, 1d

    section Phase 3: Zielsystem
    Dienste einrichten     :dienste, 12.06.2025, 1d
    Snapshot erstellen 3   :snap3, 13.06.2025, 1d

    section Phase 4: Migration
    DB migrieren           :mig, 19.06.2025, 2d
    wpconfig anpassen      :cfg, 26.06.2025, 1d
    Domain ersetzen        :dom, 27.06.2025, 1d

    section Phase 5: Tests & Abschluss
    Tests ausfuehren       :tests, 03.07.2025, 1d
    Backup autom           :auto, 10.07.2025, 1d
    Abschluss              :milestone, done, 11.07.2025, 0d
