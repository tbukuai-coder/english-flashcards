# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

A static English-vocabulary flashcard app (`index.html`) with all word data in `data/` as JSON, fetched at startup. Zero dependencies, no build step. It IS its own git repo, pushed to github.com/tbukuai-coder/english-flashcards and served via GitHub Pages (main branch root): https://tbukuai-coder.github.io/english-flashcards/

Because data loads via `fetch()`, the app only works over HTTP — never test by opening `index.html` from disk; run `python3 -m http.server` in this directory instead.

## Word data: format and how to replace it

`data/levels.json` is the manifest — an ordered array (order = home-screen order):

```json
{"id": "primary", "emoji": "🌱", "name": "小学", "en": "Primary", "desc": "基础生活词汇", "file": "primary.json"}
```

Each `file` points to a word list in `data/`: a JSON array of entries with **all five fields required**:

```json
{"w": "apple", "p": "n.", "zh": "苹果", "ex": "I eat an apple every day.", "xz": "我每天吃一个苹果。"}
```

- `w` — the English word. **This is the localStorage progress key**: renaming or removing a word silently discards learners' progress for it, and duplicate `w` values within a level corrupt stats. Keep words stable when editing; prefer appending.
- `p` — part of speech (`n.`, `v.`, `adj.`, `adv.`, or combined like `n./v.`).
- `zh` — Chinese meaning; multiple senses separated by `；`.
- `ex` / `xz` — one short example sentence and its Chinese translation. Keep `ex` simple and level-appropriate (it is read aloud by TTS and shown on the card back).

**To add/replace words in a level**: edit that level's JSON file only. No code changes.

**To add a new level** (e.g. IELTS): create `data/<id>.json` with word entries, add a manifest record to `data/levels.json`. The app renders it automatically. The `id` namespaces progress in localStorage, so changing an existing level's `id` resets that level's progress.

**Levels need ≥4 words** (the quiz draws 3 wrong options from the same level); realistically keep ≥12 so study sessions fill.

## Verifying changes

There is no test suite. Before committing data changes, run:

```bash
python3 - <<'EOF'
import json
man = json.load(open('data/levels.json'))
for m in man:
    deck = json.load(open('data/' + m['file']))
    words = [e['w'] for e in deck]
    assert len(words) == len(set(words)), f"duplicate words in {m['file']}"
    for e in deck:
        assert all(e.get(k) for k in ('w','p','zh','ex','xz')), f"missing field: {e}"
    print(m['id'], len(deck), 'words OK')
EOF
```

If `index.html` itself changed, also `node --check` the extracted `<script>` block, then smoke-test over `python3 -m http.server` (page 200, `data/levels.json` reachable). After pushing, GitHub Pages redeploys in ~1 minute; confirm with `curl -s https://tbukuai-coder.github.io/english-flashcards/data/levels.json`.

## Conventions

- UI text is Chinese-first (the audience is Chinese students); keep new UI strings consistent.
- Commit as `tbukuai-coder <tbukuai@gmail.com>` (already set repo-locally).
- Update `README.md` alongside any change to the data format or level list.
