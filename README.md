<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/brand/brainlife-mark-on-dark.svg">
    <img src=".github/brand/brainlife-mark-on-light.svg" alt="brainlife.io" width="140">
  </picture>
</p>

<h1 align="center">brainlife.io documentation</h1>

<p align="center">
  How to use brainlife.io: upload, analyze, share and publish brain data across scales.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-active-brightgreen.svg" alt="Status: active">
  <a href="https://brainlife.io"><img src="https://img.shields.io/badge/brainlife.io-site-3b9fd0.svg" alt="brainlife.io"></a>
  <a href="https://brainlife.io/docs"><img src="https://img.shields.io/badge/Docs-brainlife.io%2Fdocs-005981.svg" alt="Documentation"></a>
  <a href="https://doi.org/10.1038/s41592-024-02237-2"><img src="https://img.shields.io/badge/DOI-10.1038%2Fs41592--024--02237--2-e57536.svg" alt="Nature Methods paper"></a>
</p>

## 🧠 At a glance

| | |
|---|---|
| 🧭 **What** | Source of the brainlife.io user documentation: getting started, uploading data (ezBIDS, `bl aimport`), Apps and pipelines, visualization, publishing, the CLI, tutorials, policies and technical notes |
| 🌐 **Published at** | [brainlife.io/docs](https://brainlife.io/docs) |
| 🧱 **Stack** | [MkDocs](https://www.mkdocs.org/) with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/); pages are Markdown in [`docs/`](docs), navigation in [`mkdocs.yml`](mkdocs.yml) |
| ✅ **CI** | codespell on every pull request |
| 🧪 **Next** | [docs-next](https://github.com/brainlife/docs-next) renders the same content with Next.js |

## 🚀 Quick start

```bash
git clone https://github.com/brainlife/docs && cd docs
pip install mkdocs mkdocs-material fontawesome_markdown
mkdocs serve                  # http://127.0.0.1:8000, live reload
```

`npm run dev` runs the same `mkdocs serve`; `mkdocs build` writes the static site to `site/`. Production is deployed by the maintainers with `npm run deploy-prod` (build + rsync to brainlife.io).

## ✍️ Editing a page

1. Find the page under [`docs/`](docs) (for example `docs/using_ezBIDS.md`, `docs/using_aimport.md`, `docs/user/`, `docs/apps/`, `docs/cli/`, `docs/tutorial/`).
2. Edit the Markdown; add new pages to `nav` in [`mkdocs.yml`](mkdocs.yml). Images go in `docs/img/`.
3. Check the page with `mkdocs serve`, then open a pull request.

## 🗂️ Sections

| Folder | Contents |
|---|---|
| `docs/user/` | Using brainlife.io: projects, data, processing, visualization, publications |
| `docs/apps/` | Writing and registering Apps |
| `docs/cli/` | Command-line access |
| `docs/tutorial/` | Step-by-step tutorials |
| `docs/technical/` | Architecture, API and ezBIDS technical notes |
| `docs/resources/` | Registering your own compute resources |

## 🔗 Related repositories

| Repository | Role |
|---|---|
| [docs-next](https://github.com/brainlife/docs-next) | Next.js version of these docs |
| [ezbids](https://github.com/brainlife/ezbids) | Neuroscience data to BIDS, from instruments to shared datasets |
| [pybrainlife](https://github.com/brainlife/pybrainlife) | Python library and CLI (`bl aimport` on branch `enh/cli`) |
| [abcd-spec](https://github.com/brainlife/abcd-spec) | How brainlife Apps are written |

## 👥 Contributors

Soichi Hayashi, Bradley Caron, Franco Pestilli, Sophia Vinci-Booher, Dan Levitas and the brainlife.io team.

## 📚 Citing

If you use brainlife.io in your work, please cite:

> Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)

## 💰 Funding

brainlife.io is developed by the Pestilli Lab at The University of Texas at Austin and is publicly funded; acknowledging that support helps keep it running.

<details>
<summary>Grants (verified list, from <a href="https://github.com/brainlife/ezbids/blob/master/CREDITS.md#funding">ezbids CREDITS.md</a>)</summary>

| Funder | Award | Title |
|---|---|---|
| NIH NINDS | [U24NS140384](https://reporter.nih.gov/project-details/U24NS140384) | BRAIN CONNECTS: The Axonal Projectome EXchange (APEX) |
| NIH NIMH | [R01MH126699](https://reporter.nih.gov/project-details/R01MH126699) | Community-driven development of BIDS |
| NIH NIBIB | [R01EB030896](https://reporter.nih.gov/project-details/R01EB030896) | CRCNS US-France: open science and cloud computing of MEEG |
| NIH NIBIB | [R01EB029272](https://reporter.nih.gov/project-details/R01EB029272) | CRCNS US-France: lowering the barrier of entry to network neuroscience |
| NSF | [BCS-1636893](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893), [2148729](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729) | BD Spokes: Advanced Computational Neuroscience Network |
| NSF | [BCS-1734853](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853), [2203524](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524) | NCS-FO: Connectome mapping algorithms |
| NSF | [IIS-1912270](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270), [2148700](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700) | CRCNS US-German: DataLad |
| NSF | [ACI-1916518](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518) | BD Hubs: Midwest Big Data Hub |
| Wellcome Trust | [226486](https://grantnav.threesixtygiving.org/grant/360G-Wellcome-226486_Z_22_Z) | Developing an international governance framework to advance global brain and mental health data equity and innovation |

</details>

## 🤝 Contributing

Issues and pull requests are welcome in this repository. For help using brainlife.io, see the [documentation](https://brainlife.io/docs).

## ⚖️ License

This repository does not include a license file yet.

<p align="center"><sub>brainlife.io · Brain data across scales. One place to analyze, share and publish it. · The University of Texas at Austin</sub></p>
