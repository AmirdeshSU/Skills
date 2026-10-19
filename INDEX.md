# Skills I use

Index of the Claude Code skills and plugins I use, with their sources and pinned versions.
Nothing here is copied from upstream: each entry links to its origin and the exact
commit installed, so a fresh machine can be restored from this list.

## Plugins (user scope)

Add the marketplace once, then install the plugin: `/plugin marketplace add <repo>` then `/plugin install <plugin>@<marketplace>`.

| Plugin | Marketplace (source) | Version | Commit |
|---|---|---|---|
| caveman | caveman (`JuliusBrussee/caveman`) | 0d95a81d35a9 | 0d95a81d |
| ponytail | ponytail (`DietrichGebert/ponytail`) | 4.10.0 | e3ba2aa6 |
| ecc | ecc (`https://github.com/affaan-m/ECC.git`) | 2.2.1 | 07756cee |
| ui-ux-pro-max | ui-ux-pro-max-skill (`nextlevelbuilder/ui-ux-pro-max-skill`) | 2.13.0 | de5f12b4 |
| example-skills | anthropic-agent-skills (`anthropics/skills`) | dbd4588f9e10 | dbd4588f |
| superpowers | claude-plugins-official (`anthropics/claude-plugins-official`) | 6.4.1 | 5bf4e780 |
| frontend-design | claude-plugins-official | 8f814f49002f | 8f814f49 |
| superdesign | claude-plugins-official | 0.6.0 | a47784a8 |
| mattpocock-skills | claude-plugins-official | 1.2.3 | 959a8e9f |

```bash
/plugin marketplace add JuliusBrussee/caveman
/plugin marketplace add DietrichGebert/ponytail
/plugin marketplace add affaan-m/ECC
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
/plugin marketplace add anthropics/skills
/plugin marketplace add anthropics/claude-plugins-official
/plugin install caveman@caveman
/plugin install ponytail@ponytail
/plugin install ecc@ecc
/plugin install ui-ux-pro-max@ui-ux-pro-max-skill
/plugin install example-skills@anthropic-agent-skills
/plugin install superpowers@claude-plugins-official
/plugin install frontend-design@claude-plugins-official
/plugin install superdesign@claude-plugins-official
/plugin install mattpocock-skills@claude-plugins-official
```

## Standalone

| Skill | Source | Install |
|---|---|---|
| graphify | PyPI package `graphifyy` (v0.9.67 when this was written) | `uv tool install graphifyy` then `graphify install --platform claude` |

Gemini-backed semantic extraction needs `GEMINI_API_KEY` in the environment (never commit it).

## Project skills (SOC-Py repo)

Ten skills from `bklit/bklit-ui` live in `.agents/skills/` of the SOC-Py repo and are pinned by hash in
[`skills-lock.json`](skills-lock.json): add-x-tweet, bklit-playground, bklit-ship, bklit-studio,
bklit-studio-chart-performance, bklit-ui, pr-open, turborepo, unit-tests, wiki-llms-txt.

## My own skills (in this repo)

[`spec-before-build/`](spec-before-build) and [`spec-after-build/`](spec-after-build). The Claude app syncs
them as `anthropic-skills:spec-before-build` and `anthropic-skills:spec-after-build`.

## Synced from claude.ai

`anthropic-skills:*` (docx, pdf, pptx, xlsx, drawio, deep-research,
skill-creator, and others) are synced by the Claude app, not installed from a marketplace. They are
restored by signing in.

## Most-used in the SOC-Py project

graphify (knowledge graph), spec-after-build (as-built TRD), ponytail and caveman (output style),
superpowers brainstorming, code-review.
