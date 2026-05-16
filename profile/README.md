# In Silico Institute

We are a biomedical research institute producing open, reproducible datasets
across clinical and translational research programmes.

## Our data management approach

All datasets at the In Silico Institute are described using
[RDA maDMP](https://github.com/RDA-DMP-Common/RDA-DMP-Common-Standard)-compliant
metadata and published through an automated CI/CD pipeline that enforces our
institutional FAIR metadata policy before any record reaches a public repository.

```
researcher pushes dmp.json
        │
        ▼
┌─────────────────────┐     ┌─────────────────────┐
│  Schema check       │────▶│  Policy check        │
│  RDA maDMP schema   │     │  Institute FAIR rules│
└─────────────────────┘     └──────────┬──────────┘
                                        │ pass
                                        ▼
                             ┌─────────────────────┐
                             │  Publish to Zenodo   │
                             │  (draft created)     │
                             └─────────────────────┘
```

**Our policy requires every dataset record to have:**
- Creator ORCID identifier
- Recognised open license (SPDX / Creative Commons)
- At least one subject keyword
- Description of 150 characters or more
- Explicit access rights declaration
- Dataset identifier

## Repositories

| Repository | Purpose |
|------------|---------|
| [.github](https://github.com/in-silico-institute/.github) | Org-level reusable workflows and institutional FAIR policy |
| [fair-publish](https://github.com/in-silico-institute/fair-publish) | Python package — maDMP validation and Zenodo publishing |
| [demo-dataset](https://github.com/in-silico-institute/demo-dataset) | Example dataset repository showing the full pipeline |

## How to adopt this pipeline

Any dataset repository in this organisation needs only two files:

**`.github/workflows/validate.yml`** — triggers on every push to `dmp.json`:
```yaml
on:
  push:
    branches: ["main"]
    paths: ["dmp.json"]
jobs:
  validate:
    uses: in-silico-institute/.github/.github/workflows/fair-validate.yml@main
    with:
      dmp_path: dmp.json
```

**`.github/workflows/publish.yml`** — triggers on GitHub release.

The institutional policy is defined once in this `.github` repository and
inherited automatically by all dataset repositories.
