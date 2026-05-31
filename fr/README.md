# French — Deck Collection

French (added in **SPEC-LANG-003**) ships **inside the global Vocab-MCQ decks** rather than
as standalone per-language files. The French slice is part of the **actively-maintained,
QA-passed study set** — full A1–B2, on par with the other four languages.

> **▶ This is an active study deck.** Unlike the legacy per-language decks for the other
> languages (pending review), French is delivered through the refined global Vocab-MCQ
> Recognition/Recall decks, which are the project's primary study set.

## What you get (French slice)

| Deck | Direction | French cards | Where |
|------|-----------|------:|-------|
| **Vocab-MCQ Recognition** | definition → word (1 of 6) | 8,368 | global deck → `::French::{A1..B2}` |
| **Vocab-MCQ Recall**      | word → definition (1 of 6) | 8,368 | global deck → `::French::{A1..B2}` |

Each global `.apkg` nests French under `Vocab-MCQ-…::French::A1 … ::B2`, sorted by
frequency within each level, so you can study French alongside (or independently of) the
other languages.

## Downloads

French is included in the two global decks — import either and expand the **French** subdeck:

| Deck | Cards (all langs) | File |
|------|------:|------|
| **Vocab-MCQ-Recognition** | 34,124 | [⬇ Vocab-MCQ-Recognition.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/global/Vocab-MCQ-Recognition.apkg) (138 MB) |
| **Vocab-MCQ-Recall** | 34,124 | [⬇ Vocab-MCQ-Recall.apkg](https://storage.googleapis.com/aol-language-decks-v2/v2/global/Vocab-MCQ-Recall.apkg) (136 MB) |

SHA-256 manifest + integrity steps: see the [main README](../README.md#-integrity-verification).

## How the French deck is built

- **Word selection:** the A1–B2 set = (FLELex A–B list ∪ top-3000 frequency) ∩ rank ≤ 12,000
  → **8,368 words** (`Script/select_vocab.select_ab_words`).
- **Frequency / corpus:** OpenSubtitles 2018 — HermitDave `fr_50k` frequency list + OPUS
  mono corpus for collocations (same single source of truth as the other languages).
- **CEFR levels:** **FLELex (CEFRLex)** dominant-sense level, shown as native + CEFR
  (`A2 (FLELex)`); words off-list fall back to a corpus-calibrated frequency tier (`(freq)`).
- **Glosses:** Wiktionary-fr → Gemini fallback, normalized by the MIPROv2-tuned
  `gloss_normalizer` with a `(lemma)` label for inflected forms (e.g. `voulons` → `(vouloir) we want`).
- **Mnemonics:** sound-link memory aids (gemini-3.5-flash).
- **Example sentences:** natural 8–14-word sentence + English translation per card
  (gemini-3.5-flash, MIPROv2-tuned for accuracy + naturalness) — **99.9% coverage**.
- **Distractors:** 6-option MCQ with BGE-M3 (1024-dim) semantic neighbours.
- **Lemmas / IPA:** `simplemma` (FR) lemmatizer; `epitran fra-Latn` IPA.

## Source & licensing

- **Frequency list:** [`words/fr-frequency-list.txt`](words/fr-frequency-list.txt) — 50,000 entries, OpenSubtitles 2018 via [HermitDave/FrequencyWords](https://github.com/hermitdave/FrequencyWords) (`fr_50k`), `word count` per line.
- **CEFR overlay:** [FLELex / CEFRLex](https://cental.uclouvain.be/cefrlex/) — CC BY-NC-SA, used for personal/educational study.
- **Glosses / sentences / mnemonics:** [Google Gemini](https://ai.google.dev/) (via OpenRouter); Wiktionary-fr for first-pass glosses.
- **Distractors:** [BAAI/bge-m3](https://huggingface.co/BAAI/bge-m3) (MIT).

## How to import

1. Install [Anki Desktop](https://apps.ankiweb.net/) (free).
2. Download a global Vocab-MCQ `.apkg` above.
3. In Anki: **File → Import** → select the file, then study the **French** subdeck (A1 → B2).

> French is **global-only** for now (no standalone `fr/*.apkg`). The other deck types
> (Grammar, Conjugation, Function-Words, etc.) are not yet built for French — they will be
> aligned to these MCQ levels in a later pass.
