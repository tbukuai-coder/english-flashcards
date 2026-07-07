# 单词闪卡 · English Flashcards

A zero-dependency, single-file HTML flashcard app for learning English vocabulary with Chinese translations. Made for students at three levels:

- 🌱 **小学 Primary** — 60 basic everyday words
- 🌿 **中学 Secondary** — 64 common exam words (中考/高考 range)
- 🌳 **大学 Tertiary** — 64 academic words (CET-4/6 range)

Every word carries part of speech, Chinese meaning, and a short bilingual example sentence.

## Run it

Open `index.html` in any modern browser. No build step, no server, no dependencies. Works offline; mobile-friendly; light/dark theme follows the system.

## Modes

- **🃏 闪卡学习 (Study)** — flip cards (English front, Chinese + example back), grade yourself 认识/模糊/不认识. Uses a Leitner-style spaced-repetition system: 5 proficiency boxes with review intervals of 0/1/3/7/30 days. "不认识" resets the word to box 1 and re-queues it in the same session; a word in box 5 counts as mastered. Sessions are 12 cards: due reviews first, then new words.
- **✅ 选择测验 (Quiz)** — 10 multiple-choice questions, randomly mixed EN→中 and 中→EN, four options each. Draws from words you've already studied when possible.
- **⌨️ 拼写练习 (Spelling)** — 8 rounds of "see the Chinese, type the English word", with a first-letter hint button.

Pronunciation uses the browser's built-in Web Speech API (🔊 buttons; auto-plays on new cards and EN→中 quiz questions).

## Progress

All progress lives in `localStorage` under the key `english-flashcards-v1` (per level, per word: box number + next-due timestamp). The home screen shows mastered counts, progress bars, and how many words are due for review. "清空全部进度" in the footer resets everything.

## Extending the word lists

All data is the `DECKS` object at the top of the `<script>` block in `index.html`. Each entry:

```js
{w:"apple", p:"n.", zh:"苹果", ex:"I eat an apple every day.", xz:"我每天吃一个苹果。"}
```

Add entries to any level (or add a new level: give it a key in `DECKS` plus a matching entry in `LEVEL_INFO`). Word text (`w`) is the storage key, so renaming a word resets its progress.
