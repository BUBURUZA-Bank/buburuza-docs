# AGENTS.md — buburuza-docs

The public help site for Buburuza customers, built with Mintlify and served at
[docs.buburuza.com](https://docs.buburuza.com): account setup, sign-in, accounts and wallets, moving
money, team roles, support, security and legal. It explains what the product does for a customer, never
how the platform is built or called.

**Everything here is public.** The GitHub repository is public, and Mintlify publishes every `.md` and
`.mdx` file that `.mintignore` does not exclude — including this file. Mintlify's built-in ignore list
covers only `README.md`, `LICENSE.md`, `CHANGELOG.md` and `CONTRIBUTING.md` (plus tool directories), so
`AGENTS.md` is served at `docs.buburuza.com/AGENTS` until `.mintignore` lists it. The pull request that
changes this file must add `AGENTS.md` on its own line to `.mintignore`. Write nothing here, or anywhere
in the repository, that you would not show a customer or a competitor.

## What this repository owns

- The site's pages, navigation, theme and the `docs-safety` gate. The base branch is `main`; there is no
  `dev`. Merging into `main` deploys production.
- It does **not** hold an API reference. The developer section (`api/*.mdx` and the Developer tab) was
  removed on 2026-09-07 because it described the internal backend-for-frontend — routes, auth model,
  error codes, money-movement request shapes — which is not a published product. The canonical contracts
  live in the private architecture repository and stay there.
- Facts come from the product as it runs. Pages were last aligned against the `dev` branches of the
  BFF and web client repositories and against the production platform repository's configuration (all
  private, in the BUBURUZA-Bank organization). Re-check there before changing a statement about
  behaviour, and keep what you read there out of this repository.
- Delivery is the Mintlify GitHub app, not a workflow in this repository: every PR gets a preview
  deployment (a bot comment with the link) and Mintlify validation checks; a merge to `main` runs
  `Mintlify Deployment` to the production site.

## Repository map

| Path | What it is |
| --- | --- |
| `docs.json` | Theme, colours, fonts, navbar and the whole navigation (three tabs: Get started, Product guides, Support and legal) |
| `index.mdx`, `introduction.mdx`, `quickstart.mdx` | Overview group |
| `onboarding-and-security.mdx` | Account setup: signup, credential enrolment, verification |
| `guides/index.mdx`, `guides/account-access.mdx`, `guides/settings-and-support.mdx` | Current product guides |
| `accounts-wallets.mdx`, `payments.mdx`, `team.mdx`, `support.mdx` | Current product pages |
| `compliance-security.mdx`, `legal.mdx` | Trust and legal. `legal.mdx` states what Buburuza is and is not, who holds which permission and what is not offered; it says it governs any conflicting page |
| `guides/*.mdx` (the other twenty) | Restored draft references: title starting `Draft reference:`, description `Restored draft reference. This page is not current product guidance.`, a `<Warning>` banner, and no navigation entry |
| `.mintignore` | Files Mintlify must not publish (gitignore syntax) |
| `.github/workflows/docs-safety.yml` | The only CI workflow in the repository |
| `.github/CODEOWNERS` | `* @dklabukov @atestmirror @jacknewbile @maksimov1` |
| `README.md`, `CONTRIBUTING.md`, `LICENSE` | Repository docs, not published |

## Setup and everyday commands

There is no `package.json` or lockfile; the Mintlify CLI is installed globally.

```bash
npm i -g mint
mint dev            # local preview
mint broken-links   # dead internal links; run before every PR
```

For Mintlify component and configuration reference, the Mintlify skill can be installed with
`npx skills add https://mintlify.com/docs`.

## Writing pages

- Pages are MDX with YAML frontmatter: `title` and `description` on every page, `sidebarTitle` when the
  navigation label should be shorter than the title.
- A new page appears in the navigation only when its path (without extension) is added to a group in
  `docs.json`. A page left out of `docs.json` is **still published** at its URL: it is only left out of
  the navigation, the sitemap and site search. Leaving a page out of the navigation never hides it.
- Components already in use: `<Steps>`/`<Step>`, `<Card>`/`<CardGroup>`, `<Warning>`, `<Note>`, `<Tip>`,
  `<Tabs>`/`<Tab>`, `<AccordionGroup>`/`<Accordion>`. Anchors are `<a id="…" />` before a heading.
  Comments are `{/* … */}`.
- Internal links are root-relative without extension (`/guides/index`, `/legal`, `/support`).
- Style: active voice and second person ("you"); one idea per sentence; sentence case for headings; bold
  for UI elements (click **Settings**); code formatting for file names, commands and paths. One page, one
  topic.
- Language: English only. The English text is the source for any future localization; no Mintlify
  `languages` are configured in `docs.json`.

## What must never be published

The site describes the product as it works for customers today. Each rule below comes from a correction
already made on this site.

- **Regulated claims.** Do not describe Buburuza as a bank of any kind, and do not promise deposit
  insurance, returns on balances, investment suitability, guaranteed uptime, licences that have not been
  issued, partnerships that do not exist, or automated regulatory filing. `legal.mdx` is the reference:
  Buburuza is a financial technology company, not a bank; regulated money movement is performed by
  licensed partners; it issues no stablecoin (wallets hold third-party assets, USDC by default). The
  exact prohibited formulations are listed only in `.github/workflows/docs-safety.yml` — read them there
  and never quote them in any other file, because the scan covers every file except that workflow.
- **Anything not offered or not enabled in production.** Cards, lending and credit, interest, insurance
  and similar products are not offered. A capability that exists in code but is switched off in the
  production configuration (for example payment rails beyond those enabled there) is not described as
  available. Do not promise behaviour that production does not enforce.
- **The internal platform surface.** No endpoints, request or response shapes, error codes, bearer or
  session tokens, correlation ids, SDK or OpenAPI references, environment names, internal hostnames,
  internal service names or configuration flags. Where a page once said "available over the API", it now
  points at the dashboard the reader actually uses.
- **Credentials and identifiers.** No keys, tokens, recovery material, real account or routing numbers,
  or real customer data. Example values must be visibly fake. A live bank routing number once sat in an
  example here.
- **Unverified facts.** If a fact (a host, a limit, a count, a country list) cannot be confirmed from the
  product code or its configuration, leave it out rather than guess. Remember that a count of routes or
  features goes stale; prefer describing what the user sees.
- **Speculation and plans.** No roadmap, "coming soon" promises or unbuilt features presented as real.
  Restored drafts stay clearly labelled as drafts, out of the navigation.

## What CI enforces

| Check | Source | What it does |
| --- | --- | --- |
| `docs-safety` → `Check for forbidden patterns` | `.github/workflows/docs-safety.yml`, every PR and push to `main` | Greps the whole tree (except the workflow itself and `.git`) for credential shapes, contextual account-number and recovery-code leaks, prohibited regulated claims, and the brand-name corruption that a bad find-and-replace once caused |
| `Mintlify Validation (buburuzacorp) - link-rot` | Mintlify app, PRs | Broken links |
| `Mintlify Validation (buburuzacorp) - vale-spellcheck` | Mintlify app, PRs | Spelling; reports `neutral`, advisory |
| `Mintlify Deployment` | Mintlify app | Production deployment on `main` (skipped on PRs; their preview arrives as a bot comment) |
| `Analyze (actions)` | CodeQL default setup | Workflow code scanning |

Reproduce the safety scan by running the `run:` block of the `Scan for forbidden patterns` step with bash
from the repository root. The claim and corruption patterns are case-sensitive, the account-number and
recovery-code patterns are not; a capitalised variant of a prohibited phrase is still prohibited even if
the scan misses it.

Required checks (repository ruleset "Required status checks", on `main`): `Check for forbidden patterns`. A PR cannot merge until they pass.

## Branches, commits, pull requests

- Branch from fresh `origin/main`. Names in use: `docs/<slug>`, `chore/<slug>`, and for ticketed work
  `BBDEV-<n>-docs-<slug>`.
- Commit subjects and PR titles in use are Conventional Commits: `docs: …`, `ci: …`, `chore: …`,
  `style: …`, `fix(<scope>): …`. No commitlint runs. When the work has a Jira ticket (project `BBDEV`),
  lead with its key, as the org policy requires: `BBDEV-1647 docs: describe card controls`.
- The org ruleset on `main`: pull request required, one approval, approvals dismissed by a new push, the
  last push approved by someone other than its author, all review threads resolved, merge commit only,
  no force push. CODEOWNERS requests review from the four owners automatically; the ruleset counts an
  approval from any collaborator with write access.
- PR title and body in English; one topic per PR; open finished work ready for review, not as a draft.
  Before asking for review, open the Mintlify preview link from the bot comment and read every changed
  page there.
- **No changelog of any kind.** `CHANGELOG.md`, `changelog.d/` and any other tracked path whose name
  contains "changelog" are banned; the step `changelog is banned` in the required `Check for forbidden patterns` job fails the
  PR. The PR title and description are the record of a change.

## Gotchas

- **`AGENTS.md` is published** unless `.mintignore` lists it (see the top of this file).
- **`.mintignore` does not exclude drafts.** Its last line, `drafts/ \*.draft.mdx`, is two patterns on
  one line with a Markdown-escaped asterisk, so it matches nothing. Do not rely on a `drafts/` directory
  or a `.draft.mdx` suffix to keep a page private: fix the file (one pattern per line, no backslash) in the
  PR that first needs it, and check the preview.
- **Draft references are live.** The twenty restored drafts under `guides/` are reachable by URL (for
  example `docs.buburuza.com/guides/crypto-loans`). Keep the `Draft reference:` title, the description
  and the `<Warning>` banner on each; to withdraw one entirely, delete it or add it to `.mintignore`.
- **`support` sits in two navigation groups** (Product guides → Help and settings, and Support and
  legal → Support). Moving or renaming the page means editing both entries.
- **`README.md` is stale**: it still describes `api/*.mdx` and a Developer tab, removed on 2026-09-07.
- **Never run a global find-and-replace on the brand name.** One replaced a substring of "Buburuza" with
  Japanese kana across every code sample; the safety scan now catches that corruption.
- **`.atlas-analysis.json` is a generated summary and is gitignored**: it restated page claims as data and
  went stale on every edit. Do not commit it or any other generated restatement of the content.
- **`legal.mdx` governs.** When a product page and `legal.mdx` disagree, fix the product page, or change
  `legal.mdx` only on the basis of the current regulatory position — never to make a feature page look
  consistent.

## Definition of done

1. Every changed statement is true of the product customers use today; nothing internal, unverified,
   unreleased or regulated slipped in (re-read "What must never be published").
2. `mint dev` renders every changed page; `mint broken-links` reports nothing.
3. New pages are in `docs.json`, or are deliberately hidden drafts with the draft title, description and
   banner; nothing private relies on `.mintignore` without checking the preview.
4. The `docs-safety` step passes locally.
5. `git diff --check` clean; PR in English, ready for review; the Mintlify preview read page by page.
