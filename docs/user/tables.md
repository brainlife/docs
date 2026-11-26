# Tables

A project holds tables next to its data objects. They describe the project's subjects, sessions and files:

| Table | From |
| --- | --- |
| `participants` | `participants.tsv` / `participants.json` (one row per subject) |
| `phenotype/<name>` | BIDS `phenotype/<name>.tsv` (questionnaires, assessments, ...) |
| `samples` | BIDS `samples.tsv` |
| `sessions` | every subject's `sub-<label>_sessions.tsv`, in one table |
| `scans` | every subject's and session's `*_scans.tsv`, in one table |
| any name | any table organized like `participants.tsv`, uploaded by you |

Each row is linked to its **subject** (`participant_id`, written with or without `sub-`) and, when the table has one, its **session** (`session_id`). Rows of a scans table are also linked to their file. Data objects are linked to the same subjects, so a table row can be matched to the data of its subject.

Column definitions (units, descriptions, levels) come from the `.json` sidecars, following BIDS inheritance: a dataset-level `sessions.json` or `scans.json` describes every subject's table, and a sidecar further down (for example `sub-02/sub-02_sessions.json`) overrides it.

Tables can be read by project administrators and members.

## Getting tables into a project

- **OpenNeuro**: importing a dataset from the [Datasets](datasets.md) page brings its tables.
- **ezBIDS**: "Send to brainlife" uploads the tables of the converted dataset.
- **Command line** (`bl`, from [pybrainlife](https://github.com/brainlife/pybrainlife)):

```
bl aimport <folder> -p <project>                      # convert, deface, validate and upload, tables included
bl bids upload -p <project> <bids folder>              # upload a BIDS folder, tables included
bl table import -p <project> -d <bids folder>          # only the tables of a BIDS folder
bl table upload -p <project> -n phenotype/bdi [-k phenotype] [--json-sidecar bdi.json] bdi.tsv
```

A table you upload must be tab-separated with one header line. Name a `participant_id` column (and `session_id`, if rows are per session) so its rows are linked to subjects. Uploading a table with the same name replaces it. The participants table is edited on the project page or with the participants API, not with `bl table upload`.

## Reading tables

```
bl table list -p <project>                             # the project's tables, participants first
bl table get  -p <project> -t <table id> -o bdi.tsv    # a table as .tsv ("participants" for the participants table)
bl table remove -p <project> -t <table id>
```

## Selecting subjects and their data

Find the subjects whose table values meet some conditions, and their data objects, with `POST /api/warehouse/table/<project>/select`:

```json
{
  "where": [
    {"column": "age", "op": "gt", "value": 30},
    {"table": "phenotype/bdi", "column": "score", "op": "gte", "value": 10}
  ],
  "datatype": "<optional datatype id>"
}
```

- `table` is a table name or ID; without it, the condition is on the participants table.
- `op` is one of `eq`, `ne`, `lt`, `lte`, `gt`, `gte`, `in` (with a list as `value`).
- Conditions on the same table must be met by one row of it (for example, the same session); a subject must meet the conditions of every table.
- Values are compared as numbers when both sides are numbers, as text otherwise. A missing value (or `n/a` against a number) never matches.

The answer lists the matching subjects and their data objects (`subjects`, `datasets`, `dataset_count`).

## API

| Request | What it does |
| --- | --- |
| `GET /api/warehouse/table/<project>` | list the tables (without rows) |
| `GET /api/warehouse/table/<project>/<table id>?skip=&limit=` | a table and its rows (up to 10000 per request) |
| `GET /api/warehouse/table/<project>/subject/<subject>` | one subject's rows from every table, and its data objects |
| `POST /api/warehouse/table/<project>/select` | subjects and data objects matching conditions (above) |
| `PUT /api/warehouse/table/<project>` | create or replace a table: `{name, kind, tsv, json}` (2MB at most) |
| `DELETE /api/warehouse/table/<project>/<table id>` | remove a table |

Every request needs your brainlife.io token (`Authorization: Bearer <token>`).
