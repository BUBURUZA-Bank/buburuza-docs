# Buburuza Docs

Source of the public Buburuza documentation site (Mintlify), served at [docs.buburuza.com](https://docs.buburuza.com).

## Structure

- `docs.json` — navigation, theme, and site config
- `*.mdx` — the guide pages, all in a single Documentation tab

This site documents what the product does for the people who use it. It is not
the place for API references, internal service surfaces, or the company's
regulatory posture — see the note in Workflow below.

## Development

Install the Mintlify CLI to preview locally:

```bash
npm i -g mint
mint dev
```

Run `mint broken-links` before opening a PR to catch dead links.

## Workflow

All changes land via pull request against `main`. Merging a PR triggers a deployment. Pull requests run the `docs-safety` check. This repository backs a public site, so three kinds of content are kept out of it entirely:

- **Credentials and real identifiers** — enforced by the `docs-safety` check.
- **Internal service surfaces** — routes, auth models, and error contracts of
  services that are not published products.
- **Regulatory posture** — which registrations are held, applied for, or
  pending, and who files what with whom. Users do not need it and competitors
  should not have it.

Reviewers: see `CODEOWNERS`.
