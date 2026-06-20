# RDW kenteken afwijkingen

Open dataset met kentekens waar RDW-voertuiggegevens opvallend afwijken — met nadruk op **catalogusprijs- en cataloguswaarde-afwijkingen** in de RDW open data.

Dit repository publiceert dagelijks een JSON-export. Handig als je wilt controleren of een **catalogusprijs auto** via kenteken klopt, of als referentie bij onderwerpen als *RDW catalogusprijs klopt niet* en *cataloguswaarde auto controleren via kenteken*.

Bron-API: [https://api.scankenteken.nl/stats/suspect-plates.json](https://api.scankenteken.nl/stats/suspect-plates.json)

Het bestand `suspect-plates.json` wordt automatisch bijgewerkt via [`.github/workflows/sync-suspect-plates.yml`](.github/workflows/sync-suspect-plates.yml) (dagelijks om 11:00, Europe/Amsterdam). Handmatig syncen kan via het Actions-tabblad.

## Waarom deze dataset?

De RDW catalogusprijs (en daarmee gerelateerde BPM-berekeningen) is in de praktijk soms leeg, nul, extreem laag of opvallend hoog ten opzichte van merk, model en bouwjaar. Met open RDW-data kun je **catalogusprijs auto opzoeken** en **cataloguswaarde auto opzoeken** via kenteken — maar afwijkingen springen niet altijd direct in het oog.

Deze export verzamelt kentekens met een verdachte combinatie van technische en prijsgegevens. Veel records raken daarmee aan catalogusprijs-problemen; andere vlaggen wijzen op inconsistenties in vermogen, cilinderinhoud of massa.

## Wat zit erin?

| Veld | Type | Beschrijving |
| --- | --- | --- |
| `generated_at` | string (ISO 8601) | Tijdstip van deze snapshot |
| `threshold` | number | Drempel voor opname in de lijst |
| `count` | number | Aantal kentekens in `plates` |
| `plates` | array | Lijst met verdachte kentekens |

Per kenteken in `plates`:

| Veld | Type | Beschrijving |
| --- | --- | --- |
| `kenteken` | string | Kenteken |
| `kenteken_norm` | string | Genormaliseerd kenteken |
| `brand` | string | Merk |
| `model` | string | Model |
| `score` | number | Verdachtheidsscore (lager = verdachter) |
| `flags` | string[] | Redenen (zie hieronder) |
| `catalog_price` | number | Catalogusprijs uit de RDW-data |

### Veelvoorkomende vlaggen

| Vlag | Betekenis |
| --- | --- |
| `price_typo` | Catalogusprijs lijkt onwaarschijnlijk t.o.v. vergelijkbare voertuigen |
| `bpm_too_low` | BPM/cataloguswaarde past niet bij verwachting |
| `displacement` | Cilinderinhoud wijkt opvallend af |
| `cc_per_cyl` | Inhoud per cilinder is ongebruikelijk |
| `vmax` | Topsnelheid past niet bij motorgegevens |
| `mass_implausible` | Massa lijkt onrealistisch |

## Gerelateerd bij ScanKenteken

Meer context en uitleg over RDW catalogusprijs, cataloguswaarde en kentekencontrole: [scankenteken.nl](https://scankenteken.nl)
