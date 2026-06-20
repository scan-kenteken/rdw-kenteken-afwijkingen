# RDW kenteken afwijkingen

Open dataset met kentekens waar RDW-voertuiggegevens opvallend afwijken — vooral **catalogusprijs- en cataloguswaarde-afwijkingen** in de RDW open data.

Dagelijkse JSON-export, gesynchroniseerd vanuit de [ScanKenteken API](https://api.scankenteken.nl/stats/suspect-plates.json). Relevante zoektermen: *RDW catalogusprijs klopt niet*, *catalogusprijs auto kenteken*, *cataloguswaarde auto controleren via kenteken*, *cataloguswaarde auto opzoeken*.

## Waarom deze dataset?

De RDW catalogusprijs is in de praktijk soms leeg, nul, extreem laag of opvallend hoog ten opzichte van merk, model en bouwjaar. Deze lijst bundelt kentekens waar meerdere signalen samenkomen dat de RDW-registratie waarschijnlijk een invoerfout bevat — niet waar de auto zelf “vreemd” is.

## Velden

| Veld | Beschrijving |
| --- | --- |
| `generated_at` | Tijdstip van de snapshot |
| `threshold` | Drempel: alleen kentekens met score *onder* deze waarde |
| `count` | Aantal kentekens |
| `plates` | De lijst |

Per kenteken:

| Veld | Beschrijving |
| --- | --- |
| `kenteken` | Kenteken zoals in de RDW-bron (soms mét streepjes) |
| `kenteken_norm` | Zelfde kenteken genormaliseerd: hoofdletters, zonder streepjes of spaties — de vorm die ScanKenteken voor opzoeken gebruikt |
| `brand` / `model` | Merk en model |
| `score` | Verdachtheidsscore (lager = sterker signaal) |
| `flags` | Interne codes voor het type afwijking (zie hieronder) |
| `catalog_price` | Catalogusprijs uit de RDW-data (euro) |

`kenteken` en `kenteken_norm` zijn in de meeste records identiek; ze lopen uiteen wanneer de RDW het kenteken met streepjes opslaat (bijv. `12-AB-34` → `12AB34`).

## Afwijkingstypes (`flags`)

De codes in de JSON zijn technische interna uit de data-kwaliteitscheck. In het kort:

| Code | Wat het betekent |
| --- | --- |
| `price_typo` | Catalogusprijs sterk afwijkend t.o.v. hetzelfde merk+model, terwijl massa normaal is — typisch een extra cijfer in de prijs |
| `bpm_too_low` | BPM extreem laag t.o.v. catalogusprijs (placeholder-waarde) |
| `displacement` | Cilinderinhoud buiten realistische grenzen |
| `cc_per_cyl` | Cilinderinhoud per cilinder ongebruikelijk |
| `vmax` | Topsnelheid buiten realistische grenzen |
| `mass_implausible` | Leeg gewicht buiten realistische grenzen |

Niet elke check komt in deze lijst terecht. Alleen combinaties met een lage score (< `threshold`). Zachtere signalen — zoals een matig hoge prijs zonder bevestiging — worden wel gescoord maar niet geëxporteerd.

## ScanKenteken

Meer uitleg over RDW catalogusprijs en kentekencontrole: [scankenteken.nl](https://scankenteken.nl)
