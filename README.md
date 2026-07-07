# 单词闪卡 · English Flashcards

A zero-dependency HTML flashcard app for learning English vocabulary with Chinese translations — one `index.html` plus per-level JSON word lists in `data/`. Made for students at three levels:

- 🌱 **小学 Primary** — 60 basic everyday words
- 🌿 **中学 Secondary** — 64 common exam words (中考/高考 range)
- 🌳 **大学 Tertiary** — 64 academic words (CET-4/6 range)

Every word carries part of speech, Chinese meaning, and a short bilingual example sentence.

## Run it

Serve the folder over HTTP and open it in any modern browser — no build step, no dependencies. Live at https://tbukuai-coder.github.io/english-flashcards/ (GitHub Pages). Locally:

```bash
cd english-flashcards
python3 -m http.server 8000   # then open http://localhost:8000
```

A server is needed because the word lists load from `data/*.json` via `fetch()`, which browsers block on `file://` — double-clicking `index.html` shows a friendly error explaining this. Mobile-friendly; light/dark theme follows the system.

## Modes

- **🃏 闪卡学习 (Study)** — flip cards (English front, Chinese + example back), grade yourself 认识/模糊/不认识. Uses a Leitner-style spaced-repetition system: 5 proficiency boxes with review intervals of 0/1/3/7/30 days. "不认识" resets the word to box 1 and re-queues it in the same session; a word in box 5 counts as mastered. Sessions are 12 cards: due reviews first, then new words.
- **✅ 选择测验 (Quiz)** — 10 multiple-choice questions, randomly mixed EN→中 and 中→EN, four options each. Draws from words you've already studied when possible.
- **⌨️ 拼写练习 (Spelling)** — 8 rounds of "see the Chinese, type the English word", with a first-letter hint button.

Pronunciation uses the browser's built-in Web Speech API (🔊 buttons; auto-plays on new cards and EN→中 quiz questions).

## Progress

All progress lives in `localStorage` under the key `english-flashcards-v1` (per level, per word: box number + next-due timestamp). The home screen shows mastered counts, progress bars, and how many words are due for review. "清空全部进度" in the footer resets everything.

## Extending the word lists

All word data lives in `data/`, one JSON file per level plus a manifest:

- `data/levels.json` — the list of levels, in home-screen order. Each entry: `{"id", "emoji", "name", "en", "desc", "file"}`.
- `data/primary.json` / `secondary.json` / `tertiary.json` — arrays of word entries:

```json
{"w": "apple", "p": "n.", "zh": "苹果", "ex": "I eat an apple every day.", "xz": "我每天吃一个苹果。"}
```

**To add words**: append entries to the level's JSON file (`w`=word, `p`=part of speech, `zh`=Chinese meaning, `ex`=example sentence, `xz`=example translation). No code changes needed.

**To add a whole new level** (e.g. 雅思/IELTS): create `data/ielts.json` with word entries, then add a record to `data/levels.json` pointing at it. The app reads the manifest at startup, so the new level appears automatically.

The word text (`w`) is the progress-storage key, so renaming a word resets its learning progress.
