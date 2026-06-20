# RDW kenteken afwijkingen

Daily snapshot of suspect license plates from the ScanKenteken API.

Source: [https://api.scankenteken.nl/stats/suspect-plates.json](https://api.scankenteken.nl/stats/suspect-plates.json)

The file `suspect-plates.json` is updated automatically by [`.github/workflows/sync-suspect-plates.yml`](.github/workflows/sync-suspect-plates.yml) when the API content changes. You can also trigger a sync manually from the Actions tab.

## JSON schema

| Field | Type | Description |
| --- | --- | --- |
| `generated_at` | string (ISO 8601) | When the API generated this snapshot |
| `threshold` | number | Score threshold used to include plates |
| `count` | number | Number of plates in `plates` |
| `plates` | array | Suspect plate records |

Each item in `plates`:

| Field | Type | Description |
| --- | --- | --- |
| `kenteken` | string | Plate as stored |
| `kenteken_norm` | string | Normalized plate |
| `brand` | string | Vehicle brand |
| `model` | string | Vehicle model |
| `score` | number | Suspicion score (lower = more suspect) |
| `flags` | string[] | Reasons flagged (e.g. `displacement`, `vmax`, `price_typo`) |
| `catalog_price` | number | Catalog price used in checks |
