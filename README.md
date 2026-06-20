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
| `kenteken_norm` | Kenteken (hoofdletters, zonder streepjes) |
| `brand` / `model` | Merk en model |
| `score` | Verdachtheidsscore (lager = sterker signaal) |
| `flags` | Type afwijking (zie hieronder) |
| `catalog_price` | Catalogusprijs uit de RDW-data (euro) |

## Afwijkingstypes (`flags`)

| Vlag | Betekenis |
| --- | --- |
| `catalogusprijs_afwijkend` | Catalogusprijs sterk afwijkend t.o.v. hetzelfde merk+model, terwijl massa normaal is — typisch een extra cijfer in de prijs |
| `catalogusprijs_matig_afwijkend` | Catalogusprijs matig afwijkend *(komt zelden in export: score te hoog)* |
| `bpm_te_laag` | BPM extreem laag t.o.v. catalogusprijs (placeholder-waarde) |
| `cilinderinhoud` | Cilinderinhoud buiten realistische grenzen |
| `cilinderinhoud_per_cilinder` | Cilinderinhoud per cilinder ongebruikelijk |
| `topsnelheid` | Topsnelheid buiten realistische grenzen |
| `massa` | Leeg gewicht buiten realistische grenzen |
| `massa_volgorde` | Onmogelijke massa-volgorde (ledig > rijklaar) |

Niet elke check komt in deze lijst terecht. Alleen combinaties met een lage score (< `threshold`). Zachtere signalen worden wel gescoord maar niet geëxporteerd.

## ScanKenteken

Meer uitleg over RDW catalogusprijs en kentekencontrole: [scankenteken.nl](https://scankenteken.nl)
