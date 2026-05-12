# Rx Property Australia: Machine-Readable Content

Markdown mirror of rxproperty.com.au, served at md.rxproperty.com.au via Vercel. Built to the llmstxt.org convention so AI systems can find, fetch, and cite canonical content about Rx Property.

This repo is the source of truth for:

- `llms.txt`: curated index of canonical resources
- `llms-full.txt`: auto-generated single-file concatenation
- Per-page Markdown mirrors of selected pages on rxproperty.com.au

## What this repo serves

| File | Purpose |
|---|---|
| `llms.txt` | Index of canonical and mirrored resources, per the llmstxt.org spec |
| `llms-full.txt` | Single-file concatenation of every mirror (auto-generated) |
| `index.md` | Mirror of the home page |
| `become-a-partner.md` | Mirror of the partner / affiliate landing page |
| `listings.md` | Mirror of the listings overview |
| `faq.md` | Mirror of the FAQ page |
| `disclaimer.md` | Mirror of the disclaimer |
| `privacy-policy.md` | **Intentional stub** (see Known issues) |
| `insights/*.md` | Mirrors of individual insights articles |

Each mirror lives at `https://md.rxproperty.com.au/<path>` once deployed.

## How to add or update content

1. Create or edit the relevant `.md` file at the repo root (or under `insights/` for articles).
2. Update `llms.txt` to add or relabel the entry under the appropriate H2.
3. Commit and push to `main`.
4. The GitHub Action regenerates `llms-full.txt` automatically. Vercel redeploys both within a minute.

Brand rules: Rx casing, no em or en dashes, DD/MM/YYYY dates, Australian English, straight ASCII quotes.

## How `llms-full.txt` regenerates

The Action at `.github/workflows/regenerate-llms-full.yml` runs on every push to `main` that touches a source `.md` file or the generator script. It runs `generate-llms-full.sh`, which concatenates the canonical mirrors in the order specified in the script's `SOURCES` array, then commits the result back with the message `Auto-regenerate llms-full.txt [skip ci]`.

To run the generator locally, from the repo root:

    chmod +x ./generate-llms-full.sh
    ./generate-llms-full.sh

The script is idempotent; running it twice on the same sources produces the same output.

## How to verify changes are live

After pushing, wait roughly 60 seconds, then:

    curl https://md.rxproperty.com.au/llms.txt
    curl https://md.rxproperty.com.au/llms-full.txt
    curl https://md.rxproperty.com.au/<file>.md

If the content doesn't reflect your push, check the Vercel project dashboard for deployment status.

## Pending companion tasks in HubSpot

These live outside this repo and aren't required for the mirrors to work, but they complete the AI discoverability picture:

- 301 redirect: `rxproperty.com.au/llms.txt` to `md.rxproperty.com.au/llms.txt`
- 301 redirect: `rxproperty.com.au/llms-full.txt` to `md.rxproperty.com.au/llms-full.txt`
- Both redirects live in HubSpot URL Redirects (Settings > Tools in HubSpot CMS).

## Known issues

- `privacy-policy.md` is an intentional stub. The canonical Privacy Policy is published at rxproperty.com.au/privacy-policy and is not mirrored here because it contains a detailed vendor list. The canonical page carries `noindex, noai` directives and a copyright clause restricting AI training use.

- `insights/why-a-cold-shell-...md` is a placeholder. The canonical HubSpot page has duplicate body content (the rent article body). It will be populated once the canonical content is corrected.

## Source repository

- GitHub: https://github.com/SRPL-RXP/rxp-llms
- Deployment: Vercel project `rxp-llms`, mapped to md.rxproperty.com.au
- Maintainer: Bryce Stickland, bryce@rxproperty.com.au
