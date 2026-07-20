# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is not a code project — it is `anaka`'s personal Obsidian vault, opened as a working directory by the **Claudian** plugin (`.obsidian/plugins/realclaudian`), which embeds Claude Code inside Obsidian with file read/write, search, and bash access to this folder. There is no build, lint, or test tooling here; "operating in this repository" means reading, writing, and organizing Markdown notes and the PDFs/canvases alongside them.

## Sensitive content — read before touching files

Several top-level folders contain real personal financial records (bank/card statements, brokerage trade confirmations, insurance policies):

- `ミライノカード/`, `三井住友カード/` — credit card statement PDFs, organized by fiscal year (`２５年分`, `２６年分`)
- `日本株売買/`, `米国株売買/` — Japanese and US stock purchase/sale confirmation PDFs, named with dates (e.g. `soxl売却260514.pdf`)
- `為替管理/` — FX notes (e.g. Turkish lira position tracking)
- `保険関係/` — insurance policy PDFs

Treat these as sensitive personal data: don't summarize, copy, or transmit their contents outside the vault (e.g. into external tools, commits, or messages) unless the user explicitly asks for that in the moment. It's fine to read, index, or reference them when the user's request is about their own finances.

## Daily notes

Daily notes live directly at the vault root as `YYYY-MM-DD.md` (no `Daily Notes/` subfolder). Content is freeform — mixed research notes, market commentary, and (since 2026-07-17) structured log entries appended by an external tool (see below). When adding to a daily note, append rather than overwrite, and match the existing loose Markdown style (headers, bullet points, source links) rather than imposing a rigid template.

## External tool: daily-log-dashboard

A small standalone web app at `C:\Users\anaka\Desktop\AI作業場\daily-log-dashboard\` (outside this vault) writes to this vault's root: it appends a `## 📋 デイリーログ` block (mood, tasks, memo) to today's `YYYY-MM-DD.md` via the browser's File System Access API. It only ever touches root-level `YYYY-MM-DD.md` files — it has no knowledge of the folders above.

## Installed plugins relevant to Claude's work here

- **smart-composer** — in-vault LLM chat/composer (its index lives in `.smtcmp_json_db/` and `.smtcmp_vector_db.tar.gz`); treat these as generated cache/index data, not hand-edited content.
- **realclaudian (Claudian)** — embeds Claude Code itself in Obsidian; `.claudian/` and `.claude/agents|commands|skills/` are its config/extension points (currently empty — no custom agents, commands, or skills defined yet).
- **omnisearch**, **2hop-links-plus**, **recent-files-obsidian**, **remotely-save**, **surfing**, **typing-assistant** — editing/navigation/sync conveniences; not relevant to file-level work unless the user asks about vault configuration.

## Vault location

The vault was moved from `C:\Users\anaka\OneDrive\ドキュメント\Obsidian\Myguchi_a1` to `C:\Obsidian\Myguchi_a1` on 2026-07-17 to avoid a Chrome File System Access API write-permission issue tied to the OneDrive-synced Documents path. Obsidian's vault registry (`%APPDATA%\obsidian\obsidian.json`) points here; don't assume the OneDrive path is still valid.
