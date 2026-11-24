
# **aimport User Documentation**

**brainlife.io/aimport** is a local, agentic data importer for brainlife.io. You point it at a folder of neuroimaging data on your own computer; it works out what each scan is, converts the data to [BIDS](https://bids-specification.readthedocs.io/en/v1.11.2/), removes identifying information, defaces the anatomical images, validates the result, and only then is anything uploaded to brainlife.io.

aimport builds on [ezBIDS](using_ezBIDS.md): it reuses ezBIDS's mapping heuristics and conversion and defacing tools. The difference is where the work happens. ezBIDS runs on a server, so raw DICOM files (which carry names, IDs and dates in their headers) are uploaded before anything is removed. aimport runs everything on your computer, so raw data never leaves it.

!!! note "Status"
    The aimport **engine** is available now in the [ezBIDS repository](https://github.com/brainlife/ezbids/tree/master/aimport) as a Docker image with a small HTTP API on your own computer. The command-line front end `bl aimport`, which drives the engine and uploads the result, is available (pending release of the `bl` command-line tool). The brainlife.io/aimport web page is upcoming. You can also run the engine and drive its API directly; see the [engine README](https://github.com/brainlife/ezbids/blob/master/aimport/README.md).

## Running it

With Docker installed and `bl` logged in:

```
bl aimport <folder> --project <project id>
```

`bl aimport` starts the engine on your computer, prints the plan as a table and asks you to approve it, then writes, defaces and validates the BIDS dataset. Only a validated, de-identified result is uploaded to the project. Useful flags:

- `--target bids|datatypes`: also show the brainlife datatype of each object in the plan.
- `--deface pydeface|quickshear`: the defacing method (default `pydeface`).
- `--work <dir>` and `--keys <dir>`: the work folder and the folder for the pseudonym key. Keep the keys folder safe; it is never uploaded.
- `--tag <tag>`: add a tag to every uploaded object (repeatable).
- `--yes`: approve without asking (for scripts). `--no-upload`: stop after validation.
- `--image <ref>`: the engine image.

To use an AI model, set `AIMPORT_MODEL` and your provider's key in your environment first (see [AI models](#ai-models)). The full list of flags and an example session are in the [bl README](https://github.com/brainlife/cli#bl-aimport).

---
## What aimport does

1. **Scan.** It finds the imaging series in your folder (DICOM or NIfTI), converts them with dcm2niix and makes a first guess of each series' BIDS datatype, suffix and labels using ezBIDS's heuristics.
2. **Ask an AI agent.** An AI model reviews every series alongside ezBIDS's guess, confirms or disputes it, fills in labels such as the task name, resolves what the heuristics could not, and drafts a dataset name and README. The model only ever sees a de-identified summary (see below). Without a model configured, aimport uses the heuristics alone.
3. **Plan and review.** Everything becomes a plan that a person reviews: the BIDS name of each series, subject and session IDs, which images are defaced, which metadata files are kept, and the README. Nothing is written until the plan is approved, and any edit sets it back to not approved.
4. **Write, deface and validate.** aimport writes the BIDS dataset, defaces anatomical images, and checks the result with the BIDS validator (BIDS v1.11.2). Only a dataset that passes is ready to upload.
5. **Upload.** The validated, de-identified folder is uploaded to your brainlife.io project (by `bl aimport`, or the web page, upcoming).

## How your data is protected

- **Never leaves your computer:** the input folder (read only), the original DICOM headers and file paths, the images before defacing, and the *pseudonym key* that links real identities to the new subject IDs.
- **What the AI model sees:** only a summary of each series with acquisition parameters and descriptions, where every identifying value aimport knows about is replaced by `[x]`. Never images, raw headers or original paths. You can also use a local model (through Ollama) so nothing leaves the machine at all.
- **What is removed or changed:** identifying DICOM fields are dropped; names, IDs, birth dates and institutions are redacted wherever they also appear in descriptions, folder names, tables or README text; subjects get pseudonyms; dates are shifted by a fixed per-subject offset (intervals are kept); NIfTI header text is cleared; ages above 89 are written as 89; screenshot-style series are excluded by default because names can be burned into the pixels.
- **Defacing** is lossless outside the face: only the face voxels change.
- **The pseudonym key** is one file per project in a folder you choose. Keep it safe and backed up: it is the only way back to the real identities, it lets a later import of the same person reuse their ID, and it is never uploaded.

### Known limitations

- A name that appears **only in free text** (a README, a note) and nowhere in the headers or tables aimport reads cannot be detected automatically. Read free text during review.
- If a reviewer explicitly keeps a column that aimport drops by default (for example a notes or name column), only known identifiers are redacted in it.
- Phase 1 covers MRI and PET (anat, func, dwi, fmap, perf, pet). ASL (perf) is excluded by default unless an `aslcontext.tsv` is supplied. Microscopy (including OME-Zarr and PS-OCT), EEG, MEG and the other BIDS modalities follow in later phases.

## Metadata files

aimport also handles the non-imaging files BIDS defines: `participants.tsv`/`.json`, `phenotype/`, `sessions`, `scans` and `samples` tables, `dataset_description.json`, `README`, `CHANGES`, `LICENSE`, `CITATION.cff`, inherited sidecars and events files. Tables are de-identified rather than copied (IDs rewritten, rows of people not in the import dropped, identifying columns dropped by default, dates shifted, text redacted), and every file can be kept, edited or dropped in review. `sourcedata/` is never uploaded.

## brainlife.io datatypes

Besides a BIDS dataset, aimport can upload each series as a brainlife.io datatype object (for example `neuro/anat/t1w`, `neuro/dwi`, `neuro/func/task`, `neuro/fmap`). The mapping from BIDS to brainlife datatypes is kept in one file, [`bids_brainlife_map.yaml`](https://github.com/brainlife/ezbids/blob/master/aimport/src/aimport/data/bids_brainlife_map.yaml). Where brainlife has no datatype yet for a BIDS type, aimport keeps a draft datatype shaped like BIDS, to be registered later.

## AI models

One setting, `AIMPORT_MODEL`, picks the model: Claude (default), OpenAI models, or Qwen through Ollama (local) or DashScope. You provide your own API key. See the [engine README](https://github.com/brainlife/ezbids/blob/master/aimport/README.md#ai-model-settings) for the details.

## Learn more

- Engine README (installation, running the API, workflow, settings): [aimport/README.md](https://github.com/brainlife/ezbids/blob/master/aimport/README.md)
- Design document: [docs/superpowers/specs/2026-11-23-aimport-design.md](https://github.com/brainlife/ezbids/blob/master/docs/superpowers/specs/2026-11-23-aimport-design.md)
- Test datasets used to validate aimport: [aimport/tests/DATASETS.md](https://github.com/brainlife/ezbids/blob/master/aimport/tests/DATASETS.md)
