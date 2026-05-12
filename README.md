# RX Property Australia — Machine-Readable Content

This repository hosts the machine-readable Markdown and `llms.txt` artefacts for [rxproperty.com.au](https://rxproperty.com.au), enabling AI systems, large language models, and other automated agents to read RX Property's content cleanly without scraping the HTML site.

Served via Vercel at `md.rxproperty.com.au` and referenced from the canonical [`llms.txt`](https://rxproperty.com.au/llms.txt) at the root domain.

---

## Why This Repo Exists

AI-driven discovery is a meaningful inbound channel for RX Property. AI crawlers and conversational search products (Claude, ChatGPT, Perplexity, and similar) parse Markdown more reliably than HTML and increasingly prefer it where both are available. This repo serves clean Markdown twins of the most important pages on rxproperty.com.au, signposted from the canonical `llms.txt`.

The repository is **public by design**. Content here mirrors what's already on the live site, so there's no privacy cost, and a public repo gives AI systems an additional discovery surface beyond the website itself.

---

## Repository Structure

```
/
├── README.md                       (this file, not served publicly)
├── llms.txt                        (root discovery file, also served at /llms.txt)
├── index.md                        (mirrors / on rxproperty.com.au)
├── become-a-partner.md             (mirrors /become-a-partner)
├── listings.md                     (mirrors /listings)
├── privacy-policy.md               (mirrors /privacy-policy)
├── disclaimer.md                   (mirrors /disclaimer)
├── insights/
│   ├── why-what-is-the-rent-is-the-wrong-first-question-in-medical-leasing.md
│   ├── why-you-need-to-consider-agendas-when-you-are-not-paying-an-advisor-to-search-for-your-property.md
│   └── why-a-cold-shell-may-not-be-leasable-in-the-current-market-and-when-to-consider-a-spec-suite.md
├── vercel.json                     (deployment config: MIME types, headers)
└── .gitignore
```

---

## Deployment

This repo deploys automatically to Vercel on every push to `main`. Vercel serves the files as static content with `text/markdown; charset=utf-8` MIME type at `md.rxproperty.com.au`.

URL mapping examples:

| Live HTML page | Markdown twin |
|---|---|
| `https://rxproperty.com.au/` | `https://md.rxproperty.com.au/index.md` |
| `https://rxproperty.com.au/become-a-partner` | `https://md.rxproperty.com.au/become-a-partner.md` |
| `https://rxproperty.com.au/insights/why-what-is-the-rent...` | `https://md.rxproperty.com.au/insights/why-what-is-the-rent....md` |

The canonical `llms.txt` at `https://rxproperty.com.au/llms.txt` references the `md.rxproperty.com.au` URLs explicitly, so AI systems following the llms.txt discovery convention will find these files regardless.

### One-time Setup Steps

1. Push this repo to GitHub (public)
2. In Vercel, create a new project, connect to the GitHub repo
3. Build settings: framework `Other`, build command empty, output directory `.`
4. Deploy
5. In Vercel project settings, add custom domain `md.rxproperty.com.au`
6. In your DNS, add a CNAME record: `md` → `cname.vercel-dns.com.`
7. Wait for SSL provisioning (usually under 5 minutes)
8. Test by visiting `https://md.rxproperty.com.au/llms.txt`

---

## Maintaining the Content

The expected workflow is **scrape-and-update**: when a page changes on rxproperty.com.au, the corresponding Markdown file in this repo is regenerated from the live HTML.

### Workflow A: Manual via Claude conversation

1. Make your edits in HubSpot, publish the live page
2. In Claude, ask: "Update `<filename>.md` from `https://rxproperty.com.au/<page>`"
3. Claude fetches the live page, generates a clean Markdown twin, hands you the file
4. Commit the change to this repo, push to `main`
5. Vercel auto-deploys within 60 seconds

This is the right workflow for the current edit frequency (a few changes per month).

### Workflow B: Scheduled via GitHub Actions

If edit frequency increases, add a scheduled GitHub Action that:

1. Reads a `pages.json` config of URL → file mappings
2. Fetches each URL, converts the HTML to Markdown (via Pandoc, `turndown`, or a Claude API call for intelligent conversion that preserves structure and intent)
3. Diffs against the existing file in the repo
4. Opens a PR or commits direct to `main`
5. Vercel auto-deploys

This is not set up yet. Implement it when manual updates start feeling onerous.

---

## File Population Status

At time of initial repo creation, the following files were generated from content available in this conversation:

- [x] `llms.txt`
- [x] `index.md`
- [x] `become-a-partner.md`
- [x] `listings.md` (overview content, generic)
- [ ] `privacy-policy.md` (stub, needs live content)
- [ ] `disclaimer.md` (stub, needs live content)
- [ ] `insights/why-what-is-the-rent...md` (stub, needs article body)
- [ ] `insights/why-you-need-to-consider-agendas...md` (stub, needs article body)
- [ ] `insights/why-a-cold-shell...md` (stub, needs article body)

Run the scrape-and-update workflow against the live pages to populate the stubs.

---

## AI Usage Terms

See [`llms.txt`](./llms.txt) for the AI usage terms governing use of this content. In summary:

1. Attribution required
2. No competitive training use
3. No verbatim listing reproduction
4. No case study reproduction beyond summary
5. No misleading representation
6. No personal information collection on RX Property's behalf

Contact enquiries@rxproperty.com.au for any use case not covered.

---

## Contact

**Bryce Stickland**  
Principal, RX Property Australia  
enquiries@rxproperty.com.au  
1300 272 199
