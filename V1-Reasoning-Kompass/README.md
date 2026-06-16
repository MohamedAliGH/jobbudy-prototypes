# Prototyp V1 — „Reasoning-Kompass"

Klickbarer Vibe-Coding-Prototyp für die Discovery-Tests. Eine Datei, kein Build, keine Abhängigkeiten.

**Öffnen:** `index.html` im Browser (Doppelklick). Desktop/Laptop empfohlen — der Passungscheck ist ein Dual-Panel-Layout.

---

## Was dieser Prototyp testet

**Riskanteste Annahme:** Berufserfahrene Jobsuchende vertrauen einer *begründeten* Passungseinschätzung mehr als einer Prozentzahl — und entscheiden damit schneller und sicherer.

**Getestete Discovery-Fragen** (aus `offene_discovery_fragen.md`):
- Cluster 3: „Diese Stelle passt zu 72 % zu dir" — was hältst du davon? Welche Infos braucht eine Einschätzung für Vertrauen? Klare Empfehlung vs. Pro/Contra? Wann widersprichst du?
- Cluster 10: Welches Feedback hilft bei „über-/unterqualifiziert?" Vertraust du KI dafür?
- ICP-Validierung: Ist Fit-Confidence ein stärkerer Treiber als Anschreiben-Generierung?

## Der A/B-Kern

Rechts unten liegt die **Teststeuerung** (gestrichelt umrandet — kein Teil der Produkt-UI). Damit schaltest du den Passungscheck zwischen zwei Armen um:

| Arm | Zeigt | Prüft |
|---|---|---|
| **Reasoning** (Hypothese) | „3 Gründe dafür / 2 Punkte zu klären" + Über-/Unterqualifikations-Einordnung, kein Punktwert | Schafft Begründung Vertrauen und Entscheidungssicherheit? |
| **Score %** (Vergleich) | 78 %-Gauge + Kriterien-Balken | Erzeugt eine Zahl eher Selbstzweifel als Orientierung? |

→ Die „72 %"-Frage wird nicht gestellt, sondern **erlebt**. Beide Arme an derselben Stelle zeigen, danach laut denken lassen.

## Flow (7 Screens)

`Welcome → Onboarding (3 Rahmenbedingungen) → Stelle hinzufügen → Stellenzusammenfassung → Passungscheck (Dual-Panel, A/B) → Entscheidung (4 Wege) → Abschluss + Begleitungsfrage`

Eingebaut: sanfter Link-Fehlerfall (1. Klick auf „analysieren" im Link-Tab demonstriert die Fehlermeldung), Confidence-Prompt, 4-Wege-Entscheidung (bewerben/klären/beobachten/verwerfen), Begleitungsfrage 1–5.

## Im Test beobachten / messen

- **Entscheidungssicherheit vor/nach** (Kernmetrik, je Arm vergleichen)
- **Zeit** „Stelle eingefügt → Entscheidung getroffen"
- Reaktion **Score vs. Reasoning** beim Lauten Denken — welcher Arm baut Vertrauen?
- **Widerspruchsmomente**: Wann sagt die Person „das stimmt nicht"? Woran macht sie es fest?
- **Über-/Unterqualifikation**: Hilft die Einordnung oder verunsichert sie?
- Anteil **aktiver Entscheidungen** (100 % verwerfen = Conversion-Fail laut UX-Briefing)
- Begleitungsqualität ≥ 4/5 (Schlussfrage)

## Risiken / Annahmen

- Simulierte KI-Ausgabe ist fest auf die Beispielstelle (Sachbearbeitung IT-Koordination, Aachen) verdrahtet. Mit eigener Stelle der Testperson testet man echter — dann Reasoning vorab manuell anpassen.
- Reasoning-Arm darf nicht zur Textwand werden — kognitive Last mitbeobachten.

## Design-Bezug

Design Direction **03 „Clear Workspace"** (Navy #172A45, Teal #2F8F83, Mint, Signal-Gelb; Manrope + IBM Plex Serif). Produktprinzipien 1, 2, 7, 9, 10. Ton: ruhig, kein Druck, keine Stigmatisierung. WCAG-AA-Kontraste, Tastaturnavigation, `prefers-reduced-motion` respektiert.
