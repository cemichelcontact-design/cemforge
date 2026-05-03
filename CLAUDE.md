# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development

This is a static HTML/CSS website with no build step. To preview locally:

```bash
python3 -m http.server 8080 --bind 0.0.0.0
```

Then open `http://<vm-ip>:8080` in a browser. If the port is blocked, open it temporarily with:

```bash
firewall-cmd --add-port=8080/tcp --temporary
```

To publish changes, commit and push to `main` — the site deploys automatically via GitHub Pages to `cemforge.com`.

## Architecture

Pure static site: no framework, no bundler, no JavaScript dependencies.

- `index.html` / `contact.html` — English root pages
- `books/*.html` — Individual English book detail pages
- `de/index.html` / `de/contact.html` — German equivalents of root pages
- `de/books/*.html` — German book detail pages
- `css/styles.css` — Single shared stylesheet for all pages (EN and DE)
- `images/books/` — Book cover images; German editions are named with `DE` in the filename
- `details/books.txt` — Canonical book summaries and taglines (source of truth for copy)
- `details/links.txt` — Amazon purchase links for all editions

## Language structure

English pages live at the root; German pages live under `de/`. Path depth affects relative URLs:

| Page type | CSS path | Images path |
|---|---|---|
| Root (`index.html`) | `css/styles.css` | `images/...` |
| `books/*.html` | `../css/styles.css` | `../images/...` |
| `de/index.html` | `../css/styles.css` | `../images/...` |
| `de/books/*.html` | `../../css/styles.css` | `../../images/...` |

Only books with a German edition show the EN/DE language switcher. Echoes of the Veil → Widerhalle des Schleiers; The Chosen keeps its English title in German with subtitle "Eine Dokumentation (Aus Versehen)". Craving, In Their Place, and In Their Name have no German editions.

## Book series

- **Echoes of the Veil** series: Echoes of the Veil (book 1), Craving (book 2)
- **In Their Place** series: In Their Place (book 1), In Their Name (book 2 — English only, not yet published)

New books should be added to `index.html` in series order. Unpublished books should use placeholder text and a `href="#"` for the Amazon link, and must not be committed/pushed until ready.

## Git / deploy

The repo uses a deploy key (not a user key) scoped to this repo. The SSH host alias `github-cemforge` is configured in `~/.ssh/config` and the remote URL uses it:

```
git remote get-url origin
# git@github-cemforge:cemichelcontact-design/cemforge.git
```
