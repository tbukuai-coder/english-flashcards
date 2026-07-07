# Roadmap

Planned enhancements, roughly prioritized. Check items off as they land; keep entries short and delete stale ideas rather than letting them rot.

## Next (high value, fits current architecture)

- [ ] **错题本 (wrong-word book)** — auto-collect words answered wrong in quiz/spelling and graded 不认识 in study; a dedicated review entry on the home screen that drills only those until cleared.
- [ ] **Expand word lists toward official syllabi** — current decks are 60–64 words each; grow toward 义务教育课标 ~800 (primary), 中考 1600 / 高考 3500 core subset (secondary), CET-4/6 high-frequency (tertiary). Data-only change per `CLAUDE.md`; consider splitting big levels into units (`primary-1.json`…) with a `units` field in the manifest.
- [ ] **IPA phonetics on cards** — optional `ipa` field per word entry (e.g. `"ipa": "/ˈæp.əl/"`), shown under the word on the card front and in spelling feedback. Backward compatible: render only when present.
- [ ] **Stats page** — per-level box distribution (how many words in box 1–5), words-learned-per-day chart, and a streak calendar heatmap. All derivable from existing localStorage data plus a small daily counter.
- [ ] **Daily goal** — user-set target (e.g. 20 cards/day) with a progress ring on the home screen; ties into the existing streak check-in.

## Later (bigger lifts)

- [ ] **Listening mode** — TTS speaks the word (no text shown), student picks the meaning or types the spelling. Mostly reuses quiz/spell plumbing.
- [ ] **Reverse flashcards** — 中→英 card direction toggle in study mode.
- [ ] **Cloze exercises** — hide the word in its example sentence, student fills it in; needs no new data (examples already exist).
- [ ] **Badges/achievements** — e.g. 7-day streak, first 100 mastered, perfect quiz ×5. Pure localStorage; show on home + result screens.
- [ ] **PWA** — manifest + service worker so students can install to home screen and study offline (fetch of `data/*.json` needs a cache strategy).
- [ ] **Progress import/export** — download/upload progress as a JSON file so students can move devices; also guards against browser-data loss.
- [ ] **Custom decks** — let a teacher/parent paste or upload a word list (stored in localStorage as an extra level) without editing the repo.
- [ ] **Word images for primary level** — optional `img` field (emoji or data URI) to make young-learner cards more visual.

## Non-goals (for now)

- **Accounts / cloud sync / leaderboards** — the app is a static GitHub Pages site with no backend; everything stays client-side.
- **Recorded audio files** — Web Speech API TTS is good enough and keeps the repo tiny; revisit only if TTS quality becomes a real complaint.
- **Frameworks or build steps** — stays plain HTML/CSS/JS, one page.

## Done

- [x] Three-level decks (小学/中学/大学) with per-level JSON data + manifest (2026-07)
- [x] Leitner spaced-repetition study mode, A–D multiple-choice quiz, spelling practice (2026-07)
- [x] Playful redesign: per-level theme colors, mascot, streak check-in, star ratings, confetti (2026-07)
