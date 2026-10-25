# Diagvo vehicle generations adapted from OBDb

This folder is an **adapted dataset** of OBDb (public copy:
<https://github.com/dynaz/diagvo-vehicle-data/tree/main/obdb>), published under the
**Creative Commons Attribution-ShareAlike 4.0 International** licence
(CC BY-SA 4.0, full text in [`LICENSE`](LICENSE)).

## Attribution

Vehicle generation names and model years from **OBDb**, the community OBD
database: <https://github.com/OBDb>, licensed CC BY-SA 4.0. Each record in
`generations.json` names the exact source repository and commit, for example
`https://github.com/OBDb/Toyota-Hilux` at `b499e5b12472…`.

## What was changed

From each OBDb repository's `generations.yaml`, only generation **names and
start/end years** were taken; descriptions and signal definitions were not used.
Then:

1. **Mapped to Diagvo catalog models** (make and model ids), e.g. OBDb
   `Toyota-Hilux` to Diagvo `toyota/hilux-revo`.
2. **Restricted to one generation where the Diagvo model names a specific
   generation:**
   - Toyota Hilux Revo → Eighth Generation (AN120/AN130)
   - Toyota Hilux Vigo, Hilux Vigo Champ → Seventh Generation (AN10/AN20/AN30)
   - Toyota Hilux Tiger D4D → Sixth Generation (N140/N150/N160/N170)
   - BMW E39, E46, E60, E90 and Mercedes-Benz W203, W211 (chassis codes) →
     only the OBDb generation whose name contains that code
3. **Left out generations that ended before 1996** (before OBD-II).
4. **Added Diagvo generation ids** (`obdb-<slug of the name>`).

Not mapped: Toyota Hilux Champ and Hilux Travo (no matching OBDb generation),
Isuzu D-Max (no Isuzu repository; `Chevrolet-D-Max` was not assumed to apply).

## Signal definitions (extended data)

`assets/obdb/signals/` (published as `obdb/signals/`) holds OBDb signalsets for the mapped models: every
`signalsets/v3/*.json` file of each repository (`default` and year overrides),
reduced to the fields Diagvo decodes (command header, receive address,
service and PID, filters, signal ids, names, paths and formats);
descriptions were removed. `assets/obdb/vehicles.json` maps Diagvo `makeId/modelId` to the
repository. Built by `tool/catalog/build_obdb_signals.py`.

These definitions are community data. Diagvo has not verified them on any
vehicle and does not derive diagnostic coverage from them.

## Meaning

These records describe **which generations of a model exist and when they were
built**. They say nothing about diagnostic support: Diagvo keeps diagnostic
coverage separate, and no coverage was derived from this data.

## Files

| File | Content |
|---|---|
| `generations.json` | records with make/model/generation ids, name, years and source (repo, commit, original name, licence) |
| `generations.csv` | the same records as CSV |
| `LICENSE` | CC BY-SA 4.0 legal code (copied from the OBDb repository) |

## Reproducing

```sh
python3 -I tool/catalog/fetch_obdb.py staging/obdb/matches.json staging/obdb
python3 -I tool/catalog/build_obdb_dataset.py staging/obdb/matches.json staging/obdb \
  assets/catalog/seed_catalog.json data/obdb
```

Anyone may share and adapt this dataset under the same licence (CC BY-SA 4.0),
with attribution to OBDb and Diagvo.
