# CLAUDE.md — Förderrechner Heizung (KfW 458)

Interner Vertriebs-Grobrechner für die Heizungsförderung nach KfW 458, Rechtsstand BEG-Reform ab 21.07.2026. Repo: `suedenergie-pv/foerderrechner`. Single-File-PWA (`index.html`), Vanilla JS, kein Build-Step — gleiches Prinzip wie `wirtschaftlichkeitsrechner`.

## Fachregeln (nicht ohne Beleg ändern)
- Zuschuss = `30 % · FK  +  Zusatzsatz · (FK / n)`. Boni bemessen sich nur auf den Anteil der **selbstgenutzten** WE (FK/n).
- `Zusatzsatz = max(0, min(30 + KGB + Einkommensbonus, DECKEL) − 30)`.
- **Gate:** Förderung setzt den Ersatz einer **fossilen** Bestandsheizung voraus. Bei „Keine fossile Heizung" gibt es voraussichtlich **gar keine** Förderung (auch keine Grundförderung) → Ergebnisbereich zeigt Hinweis statt Zahl. Feld 4 (Bestandsheizung) ist deshalb **immer** Pflicht, auch für Vermieter.
- Grundförderung 30 % gilt (wenn Gate passiert) immer, auch für reine Vermieter (dann keine Boni).
- KGB nur bei Selbstnutzung **und** Bestandsheizung „Öl/Kohle/Nachtspeicher/Gasetage" (funktionstüchtig, keine Altersgrenze) **oder** „Gaszentralheizung" mit Haken „mind. 20 Jahre in Betrieb".
- Familienzuschlag: 1 minderj. Kind senkt das relevante zvE um 10.000 € → eine Einkommensstufe runter.
- DECKEL = 80 % bei relevantem zvE ≤ 30k, sonst 70 %.
- Degression: FHB_WE1 −750 € und KGB −4 %-Punkte je Halbjahr (Stichtage 01.02./01.08.), per Formel, nicht als hart endende Tabelle.
- **TODO vor Go-live:** Deckel-Regel, Familienzuschlag-Mechanik, Gate-Regel und die 20-Jahre-Grenze für Gaszentralheizungen gegen finales Merkblatt kfw.de/458 gegenprüfen (im Code als `// TODO` markiert). Wertschöpfungsbonus Q1/2027 ist nur Platzhalter.

## Verifikation
- Rechenkern gegen 10 Referenz-Testfälle (T1–T10) in `index.html`. In der Browser-Konsole: `foerderTests()` → muss `ALLE TESTS BESTANDEN` liefern.
- Dev-Server: `powershell -ExecutionPolicy Bypass -File serve.ps1` → http://localhost:3000.
- `preview_screenshot` friert in dieser Umgebung oft ein (Timeout) — Styling/Ergebnis stattdessen per `preview_inspect`/`preview_eval` prüfen.

## Design & Daten
- Internes Tooling-CI: Orange `#e8603a` auf hellem `#f4f6f8`, Tokens im `:root` von `index.html`. Mobile-first (Nutzung am Handy beim Kunden).
- **Logo: nur `suedenergie-logo-farbe.png`** (Farb-Wortmarke, vollständiges „g"). Das alte `suedenergie-logo.png` (mit abgeschnittener g-Unterlänge) ist gelöscht und darf **nie wieder** verwendet werden. Neues Logo ist ein breites Wortmark (~6,8:1) → im Header per `height` klein halten und `max-width` deckeln, sonst sprengt es die Mobile-Zeile.
- **Keine Speicherung, kein localStorage** — bewusst keine Kundendaten im Gerät. Service Worker cached nur die App-Shell.
