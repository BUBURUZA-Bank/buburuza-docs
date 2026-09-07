# Buburuza Docs

Source of the public Buburuza documentation site (Mintlify), served at [docs.buburuza.com](https://docs.buburuza.com).

## Structure

- `docs.json` — navigation, theme, and site config
- `*.mdx` — guide pages (Documentation tab)
- `api/*.mdx` — developer docs and API reference (Developer tab)

## Development

Install the Mintlify CLI to preview locally:

```bash
npm i -g mint
mint dev
```

Run `mint broken-links` before opening a PR to catch dead links.

## Workflow

All changes land via pull request against `main`. Merging a PR triggers a deployment. Pull requests run the `docs-safety` check — never commit credentials, account identifiers, or internal-only details: this repository backs a public site.

Reviewers: see `CODEOWNERS`.
