# Salesforce Renames

An [Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
v0.2 bundle tracking Salesforce product and certification renames — what
changed, when, and what to do about it. Built so an agent can answer "is
anything I hold about to change names?" without a human doing the lookup by
hand.

## Give this to your agent

> Clone `github.com/shivanshsen7/salesforce-renames`, check it against my
> Salesforce certs/products, tell me what's changing and by when.

No GitHub auth needed — public repo, plain `git clone`. An agent without
shell access can instead pull `renames.jsonl` via the GitHub API or raw
content URL and `jq`/`grep` it directly (see below).

## Structure

```
certs/retiring/    24 concepts — certifications retiring 2027-02-01
certs/renamed/     16 concepts — certifications with a cosmetic name change 2026-07-24
products/          92 concepts — product/platform rename history, 2000–present
renames.jsonl      all 132 concepts flattened to one JSON-LD node per line
```

## Querying without cloning

```sh
# All entries mentioning "Sales Cloud" anywhere
curl -s https://raw.githubusercontent.com/shivanshsen7/salesforce-renames/main/renames.jsonl \
  | grep -i "sales cloud"

# Just the certs retiring, as a table of name + deadline
curl -s https://raw.githubusercontent.com/shivanshsen7/salesforce-renames/main/renames.jsonl \
  | jq -c 'select(.["@type"]=="RetiringCertification") | {certName, retirementDate}'
```

## Provenance and trust

- **`certs/`** is sourced directly from Salesforce's own official FAQs (see
  each concept's `sources`) — one-to-one with the primary source, not a
  secondhand recap.
- **`products/`** is sourced from the [renameforce.com](https://renameforce.com)
  community dataset, cited per concept. It is **not** independently verified
  entry-by-entry — treat it as a good-faith community record, not an
  official Salesforce statement. Each concept's `sources` field says so
  explicitly per OKF's provenance convention.
- Every concept carries `generated: {by: claude-sonnet-5/claude-code, ...}`. A
  follow-up automated fact-check pass (`claude-haiku-4-5/fact-check-agent`, one
  agent per product category plus one for all 40 certifications) independently
  web-searched for a second source per entry: **123 of 132 concepts** now carry
  a `verified` field and a second `sources` entry, moving them to
  **machine-confirmed** trust tier — still not the same as **human-reviewed**,
  since no person has checked these line-by-line yet.
- **4 discrepancies were found and flagged, not resolved** — each concept has
  a "Discrepancy flagged" body section and stayed `unverified` rather than
  being marked confirmed despite having a second source, since that source
  *contradicts* the original claim rather than confirms it. See `log.md` for
  the full list. The remaining unflagged entries stayed `unverified` simply
  because no independent source was found in the time budget, not because
  anything is wrong with them.
- If you spot an error, open an issue or a PR — a human review pass will
  upgrade entries to `verified: {by: human:..., at: ...}` as it happens.

## Retiring certifications (24)

- [Advanced Field Service Accredited Professional](certs/retiring/advanced-field-service-accredited-professional.md)
- [Salesforce Certified B2B Solution Architect](certs/retiring/salesforce-certified-b2b-solution-architect.md)
- [Salesforce Certified B2C Commerce Architect](certs/retiring/salesforce-certified-b2c-commerce-architect.md)
- [Salesforce Certified Consumer Goods Cloud: Trade Promotion Management Accredited Professional](certs/retiring/salesforce-certified-consumer-goods-cloud-trade-promotion-management-accredited-professional.md)
- [Contact Center Accredited Professional](certs/retiring/contact-center-accredited-professional.md)
- [Salesforce Certified CPQ Administrator](certs/retiring/salesforce-certified-cpq-administrator.md)
- [CPQ and Billing Consultant Accredited Professional](certs/retiring/cpq-and-billing-consultant-accredited-professional.md)
- [Salesforce Certified Industries CPQ Developer](certs/retiring/salesforce-certified-industries-cpq-developer.md)
- [Salesforce Certified Education Cloud Consultant](certs/retiring/salesforce-certified-education-cloud-consultant.md)
- [Energy and Utilities Cloud Accredited Professional](certs/retiring/energy-and-utilities-cloud-accredited-professional.md)
- [Heroku Developer Accredited Professional](certs/retiring/heroku-developer-accredited-professional.md)
- [Loyalty Management Accredited Professional](certs/retiring/loyalty-management-accredited-professional.md)
- [Salesforce Certified Marketing Cloud Account Engagement Consultant](certs/retiring/salesforce-certified-marketing-cloud-account-engagement-consultant.md)
- [Marketing Cloud Advanced Cross Channel Accredited Professional](certs/retiring/marketing-cloud-advanced-cross-channel-accredited-professional.md)
- [Marketing Cloud Intelligence Accredited Professional](certs/retiring/marketing-cloud-intelligence-accredited-professional.md)
- [Marketing Cloud Personalization Accredited Professional](certs/retiring/marketing-cloud-personalization-accredited-professional.md)
- [Media Cloud Accredited Professional](certs/retiring/media-cloud-accredited-professional.md)
- [Salesforce Certified MuleSoft Catalyst Consultant](certs/retiring/salesforce-certified-mulesoft-catalyst-consultant.md)
- [Salesforce Certified Mulesoft Hyperautomation Developer](certs/retiring/salesforce-certified-mulesoft-hyperautomation-developer.md)
- [Net Zero Cloud Accredited Professional](certs/retiring/net-zero-cloud-accredited-professional.md)
- [Order Management Administrator Accredited Professional](certs/retiring/order-management-administrator-accredited-professional.md)
- [Order Management Developer Accredited Professional](certs/retiring/order-management-developer-accredited-professional.md)
- [Process Automation Accredited Professional](certs/retiring/process-automation-accredited-professional.md)
- [Salesforce Certified Nonprofit Success Pack Consultant](certs/retiring/salesforce-certified-nonprofit-success-pack-consultant.md)

## Renamed certifications (16)

- B2B Commerce for Administrators Accredited Professional → **Salesforce Accredited B2B Commerce Administrator Professional**
- B2B Commerce for Developers Accredited Professional → **Salesforce Accredited B2B Commerce Developer Professional**
- Communications Cloud Accredited Professional → **Salesforce Accredited Agentforce Communications Professional**
- Consumer Goods Cloud Accredited Professional → **Salesforce Accredited Agentforce Consumer Goods Professional**
- Financial Services Cloud Accredited Professional → **Salesforce Accredited Agentforce Financial Services Professional**
- Health Cloud Accredited Professional → **Salesforce Accredited Agentforce Health Professional**
- Manufacturing Cloud Accredited Professional → **Salesforce Accredited Agentforce Manufacturing Professional**
- Public Sector Solutions Accredited Professional → **Salesforce Accredited Agentforce 360 for Public Sector Professional**
- Salesforce Certified B2C Commerce Cloud Developer → **Salesforce Certified B2C Commerce Developer**
- Salesforce Certified Field Service Consultant → **Salesforce Certified Agentforce Field Service and Operations Consultant**
- Salesforce Certified Marketing Cloud Email Specialist → **Salesforce Certified Marketing Cloud Engagement Specialist**
- Salesforce Certified Nonprofit Cloud Consultant (NPC) → **Salesforce Certified Agentforce Nonprofit Consultant**
- Salesforce Certified Revenue Cloud Consultant → **Salesforce Certified Revenue Management Consultant**
- Salesforce Certified Sales Cloud Consultant → **Salesforce Certified Agentforce Sales Consultant**
- Salesforce Certified Sales Foundations → **Salesforce Certified Agentforce Sales Foundations**
- Salesforce Certified Service Cloud Consultant → **Salesforce Certified Agentforce Service Consultant**

## Products

See `products/index.md` for the full list of 92, or grep `renames.jsonl` for
`"@type":"ProductRenameHistory"`.
