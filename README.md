# Förderrechner Heizung (KfW 458)

Internes Vertrieblertool für eine schnelle, **unverbindliche Grobauskunft** zur Heizungsförderung nach KfW 458, Rechtsstand der BEG-Reform ab **21.07.2026**.

- Single-File-PWA (`index.html`), Vanilla JS, kein Build-Step, kein Framework.
- Manifest + Service Worker für Offline-Betrieb und Installierbarkeit (App-Shell wird gecacht).
- **Keine Datenspeicherung, kein localStorage** – bewusst keine Kundendaten im Gerät. Ergebnis nur am Bildschirm, keine PDF-Ausgabe.
- Design: internes Tooling-CI (Orange `#e8603a` auf hellem `#f4f6f8`), mobile-first.

## Lokal starten

```powershell
powershell -ExecutionPolicy Bypass -File serve.ps1
```

Dann `http://localhost:3000` öffnen. (Ein einfacher statischer Server genügt; die App braucht keinen Backend-Dienst.)

## Rechenlogik / Testfälle

Der Rechenkern und die 10 Referenz-Testfälle (T1–T10) stehen in `index.html`. In der Browser-Konsole prüfbar mit:

```js
foerderTests()
```

Erwartet: `ALLE TESTS BESTANDEN`.

## Scope V1

**Drin:** ein Wohngebäude mit zentraler Heizungsanlage, ein Antragsteller (Eigentümer); EFH und MFH inkl. teilvermieteter Gebäude (Selbstnutzung einer WE); reine Vermieterfälle (nur Grundförderung 30 %); Degression ab 2027 über das Antragsdatum.

**Draußen (im UI als Hinweis genannt):** WEG-Fälle und Etagenheizungen; Ergänzungskredit (KfW 358/359); Wertschöpfungsbonus ab Q1/2027 (nur als Code-Platzhalter); technische Förderfähigkeit der Anlage (JAZ, Kältemittel etc.) wird nicht geprüft. Fossile Anteile von Hybridanlagen sind nie förderfähig.

## Rechtsstand / offene Punkte

Vor Go-live gegen das **finale KfW-Merkblatt 458** gegenprüfen – insbesondere:

- **Gate-Regel:** Ohne Ersatz einer fossilen Bestandsheizung voraussichtlich gar keine Förderung.
- **20-Jahre-Grenze** für Gaszentralheizungen (Eckpunkte nennen sie nicht explizit; bis dahin alte Merkblatt-Logik).
- **Deckel-Regel** (80 % bei relevantem zvE ≤ 30k, sonst 70 %).
- **Familienzuschlag-Mechanik** (Kind senkt das relevante zvE um 10.000 €).

Alle sind im Code mit `// TODO` markiert.

## Quellen

- KfW-Pressemitteilung 08.07.2026 „Anpassungen in den KfW-Produkten der BEG"
- BMWE-Eckpunkte / Pressepapier „Reform der Gebäudeförderung"
- [kfw.de/458](https://www.kfw.de/458) – finales Merkblatt bei Veröffentlichung gegenprüfen
