<p align="center">
  <img src="./assets/readme/hero.svg" width="100%"
       alt="Salesforce Renames: an Open Knowledge Format v0.2 bundle. Real example — sales-cloud.md's timeline: salesforce.com (SFDC) in 2000, renamed to Sales Cloud in 2009, renamed to Agentforce Sales in 2025. 132 concepts across certs and products, queryable via renames.jsonl.">
</p>

## Give this to your agent

> Clone `github.com/shivanshsen7/salesforce-renames`, check it against my
> Salesforce certs/products, tell me what's changing and by when.

No GitHub auth needed — public repo, plain `git clone`. No shell access?
Pull `renames.jsonl` straight off the GitHub API or raw content URL and
`jq`/`grep` it instead:

```sh
# All entries mentioning "Sales Cloud" anywhere
curl -s https://raw.githubusercontent.com/shivanshsen7/salesforce-renames/main/renames.jsonl \
  | grep -i "sales cloud"

# Just the certs retiring, as a table of name + deadline
curl -s https://raw.githubusercontent.com/shivanshsen7/salesforce-renames/main/renames.jsonl \
  | jq -c 'select(.["@type"]=="RetiringCertification") | {certName, retirementDate}'
```

## Why trust it

<p align="center">
  <img src="./assets/readme/verification.svg" width="100%"
       alt="Two sources — Salesforce's official FAQs and the renameforce.com community dataset — feed 7 independent fact-check agents. Of 132 concepts: 123 machine-confirmed, 4 flagged with a real discrepancy, 5 unverified with no second source found.">
</p>

This isn't one dataset taken at face value. `certs/` comes straight from
Salesforce's own official FAQs. `products/` comes from the
[renameforce.com](https://renameforce.com) community dataset — cited as a
source, not silently absorbed as primary fact. Then 7 independent agents
went looking for a second, unrelated source for every single entry:

- **123 of 132** now carry a second source and a `verified` field —
  *machine-confirmed*, a step up from community-sourced, though still short
  of a human review.
- **4 real discrepancies were caught and flagged in place** rather than
  quietly marked confirmed — Audience Studio's actual retirement year,
  Salesforce Functions' actual retirement date, Nonprofit Cloud's actual
  current name, and Flow Builder's earliest recorded name all disagree with
  a second source. See `log.md` for the exact notes.
- The remaining 5 just had no independent source turn up in the search
  budget — that's "unchecked," not "wrong."

## How to navigate this repo

| You want... | Go to |
|---|---|
| Raw data to `grep`/`jq` — no cloning, no markdown parsing | [`renames.jsonl`](renames.jsonl) |
| To browse certifications retiring Feb 2027 | [`certs/retiring/`](certs/retiring/) (24, each its own file) |
| To browse certifications with a cosmetic name change | [`certs/renamed/`](certs/renamed/) (16) |
| To browse product/platform rename history since 2000 | [`products/`](products/) (92) |
| The strict [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md) v0.2 entry point | [`index.md`](index.md) |
| The dated history of what changed in this bundle | [`log.md`](log.md) |

Each subdirectory has its own `index.md` — GitHub renders it automatically
when you click into the folder, so `products/` already gives you a full
table of contents without this README repeating all 92 names.

## Structure

```
index.md            OKF-spec entry point (progressive disclosure, no frontmatter)
log.md               dated update history
renames.jsonl        all 132 concepts, one JSON-LD node per line
certs/retiring/       24 concepts — certifications retiring 2027-02-01
certs/renamed/        16 concepts — certifications with a cosmetic name change 2026-07-24
products/             92 concepts — product/platform rename history, 2000–present
```

Every concept is a plain markdown file: YAML frontmatter (only `type` is
required by spec) plus a normal markdown body. `sources`, `generated`, and
`verified` are what make the trust story above queryable rather than just
asserted in this paragraph — open any file in `products/` or `certs/` to
see them directly.

## Correcting or extending this

Spot an error, or know about a rename this bundle is missing? Open an issue
or a PR. A human review pass will upgrade entries from `machine-confirmed`
to `verified: {by: human:..., at: ...}` as it happens.
