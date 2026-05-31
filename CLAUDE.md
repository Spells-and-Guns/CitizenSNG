# CLAUDE.md

This is **CitizenSNG**, the Spells & Guns (SNG) fork of the Citizen MediaWiki skin.

Upstream's own agent documentation applies in full — coding conventions, verification
commands, caching rules, i18n, etc. It is imported below. Read it.

@AGENTS.md

---

## This fork (read before editing)

CitizenSNG is a thin fork of [Citizen](https://github.com/StarCitizenTools/mediawiki-skins-Citizen).
It powers the spellsandguns wiki farm and is the only skin loaded
(`wfLoadSkin( 'CitizenSNG' )` in `LocalSettings.php`; `$wgDefaultSkin = "citizen"`).

**The fork stays as close to upstream as possible so updates merge cleanly.**
All internal identifiers are deliberately left **identical to upstream**:

- skin id: `citizen` (not `citizensng`)
- PHP namespace: `MediaWiki\Skins\Citizen\`
- ResourceLoader modules: `skins.citizen.*`
- i18n keys: `citizen-*`

Only three things differ from upstream: the folder name (`CitizenSNG/`), the
`skin.json` `"name"` field (`CitizenSNG`, display-only), and the `wfLoadSkin` line.
**Do not "rename" these identifiers to match the fork** — doing so would make every
upstream merge conflict on thousands of lines, for zero functional gain.

## Upstream

- Remote: `upstream` → `https://github.com/StarCitizenTools/mediawiki-skins-Citizen.git`
- Pull updates: `git fetch upstream --tags` then `git merge <tag>` (e.g. `git merge v3.17.0`).
- Current base: track the latest `v*` tag; the fork is otherwise stock Citizen.

## Isolate your changes

The whole point of the fork structure is a **tiny diff against upstream**. Keep it that way:

- **CSS/LESS** — put ALL local styling in `resources/skins.citizen.styles/sng/overrides.less`
  (and other files under `sng/`). It is imported **last** from `skin.less`, so its rules win
  over upstream without `!important`. Do **not** scatter edits across upstream LESS files.
- Prefer overriding the **CSS custom properties** from `tokens-citizen.less`
  (e.g. `--color-surface-0`, `--background-color-*`) — one change cascades through the whole skin.
- New assets (background images, logos) go under `sng/` or a clearly-SNG path, never mixed
  into upstream asset folders.

## PHP / template edits

Sometimes CSS isn't enough and you must touch an upstream `.php` or `.mustache` file.
When you do:

- Keep the edit **surgical** — change the fewest lines possible.
- **Comment every SNG-specific line or block with `// SNG:`** (or `{{!-- SNG: --}}` in
  Mustache, `/* SNG: */` in LESS). This makes your changes instantly visible when resolving
  a merge conflict during an upstream pull.
- Avoid reformatting or moving surrounding upstream code — it inflates the diff and causes
  conflicts.

## Before committing

Run the relevant checks from `AGENTS.md` (e.g. `npm run lint:styles` for LESS changes).
Use Conventional Commits; no emojis (a hook adds them).
